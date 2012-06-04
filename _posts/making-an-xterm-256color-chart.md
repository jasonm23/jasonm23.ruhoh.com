---
title: Making an xterm-256color chart
date: '2012-06-04'
description: in which we make a nice xterm color chart with SVG.
layout: post
---

I generally have a few terminal windows open, and I also tend to jump around a lot of locations on ssh, either in my home, or somewhere else in the world.

I find it's useful to color parts of my `zsh` prompt differently for each box I commonly use, so I can see/feel at a glance where I am.

Obviously the choices of colors for regular xterm are limited to the fairly ugly 16, and retinting those on Terminals that support such things is a bit of a maintenance headache that no one should endure.

Terminals which support xterm-256color are plentiful enough for me for that to be a workable arrangement.

I'm a visually driven beast, so after fumbling about with ansi echo loops to find a approiate colors (googling xterm-256color chart didn't occur to me), I decided I'd like to make a couple of helpful xterm-256color thingoes.

So first up we need a dataset, I pulled these color mappings out of  [Here](http://www.frexx.de/xterm-256-notes/data/xterm-colortest) and here they are Yaml format, I've re-arranged them to match the table [on Wikipedia](http://en.wikipedia.org/wiki/File:Xterm_color_chart.png), because I guess I'm going to donate the SVG output there as a public domain image, and wasn't sure what the hue spread algorithm used there was.

Ok, so with our dataset, we can make our chart. We're using the same 80x60 box as the png, the xterm number sits in the bottom left corner and the hex color will sit near the vertical and horizontal center, we can use fixed postions for these since the strings are all the same size. The font is bitstream-vera-sans-mono for portablilty/licencing freedom.

I used the handy `text_color` function in the `colorist` gem (`gem install colorist`) which is a nice little library of color functions...

    require 'colorist'   
    include Colorist
   
    puts "#434392".to_color.text_color

Give you the text color from a CSS valid color string.

Ok, so let's have a look at laying this out... As I mentioned there's some nice color coordination on the origial table, which I want to bring over, sorting the big color list into the appropriate chunks does the trick. [You can see the color data here](https://raw.github.com/gist/2868981/b80871f36954f11c7a7ee95a2d8d4f9886ce7361/xterm-256color.yaml)
  
Then we divide into base (16), greyscales (24), and the rest... (216).

The 24 greyscales looked nice arranged as a snaked pair of rows, 
   
    black -> mid grey
    white <- mid grey
    
So we just reverse the last 12 so we can just do a simple modular arithmetic grid.

    x = i%12 * w
    y = i/12 * h

We do something similar for the base (16) colors, using `%8` instead.

It's a bit more tricky to layout the 216 colors into 3 equal tables.

Let's have a look at it...

    x = i/6 * w % table_width
    y = i%6 * h + table_height * (i/cells_per_table)   

By the way this set of tables order by columns not rows, so we do modulo on 6 cells per column, each table has 72 cells, and we use the a modulo on the table width to reset the x position. 

These sorts of loops are more fun to step through, and watch them build, the math becomes much clearer, modulo is sweet.


    ## quick dirty script to render a xterm256 color chart in svg.
    require 'yaml'
    require 'colorist'
    include Colorist
    
    def svg_box x, y, s, w, h
      xterm, hex = s.split ","
      text = "##{hex}".to_color.text_color 0.6
      <<-SVGBOX
      <g transform="translate(#{x},#{y})">
        <rect x="0" y="0" width="#{w}" height="#{h}" style="fill:##{hex};" />
        <text x="#{w/2}" y="#{(h/2)+2}" style="fill:#{text};text-anchor:middle;" >#{hex}</text>
        <text x="2" y="#{h-3}" style="fill:#{text};" >#{xterm}</text>
      </g>
      SVGBOX
    end
    
    xtermcolors = YAML.load_file "xtermcolors.yaml"
    rows = 6
    columns = 12
    cells_per_table = columns * rows
    w = 80
    h = 60
    table_spacing = 20
    table_height = rows * h + table_spacing 
    table_width = columns * w
    doc_height = (table_height *3) + (h*4) + table_spacing
    boxes256 = []
    boxesGrey = []
    boxes16 = []
    
    xtermcolors[:xterm256].each_with_index do |s,i|
      x = i/rows * w % table_width
      y = i%rows * h + table_height * (i/cells_per_table)   
      boxes256 << svg_box(x, y, s, w, h)
    end
    
    xtermcolors[:xtermGreyscale].each_with_index do |s,i|
      x = i%12 * w
      y = i/12 * h
      boxesGrey << svg_box(x, y, s, w, h)
    end
    
    xtermcolors[:xterm16].each_with_index do |s,i|
      x = i%8 * w
      y = i/8 * h
      boxes16 << svg_box(x, y, s, w, h)
    end
    
    svg = <<-SVG
    <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="#{table_width}" height="#{doc_height}">
      <style>
         /* rect { stroke: black; stroke-width:0.15pt; } */
         text {
           font-size:9pt; 
           font-family: "Bitstream Vera Sans Mono", "Droid Sans Mono", "Menlo", "Monaco", "Consolas", "Inconsolata", "Courier New"; 
           fill: white;
         }
         .head {
           font-family: 'Helvetica Neue', Helvetica, Calibri, Arial, sans-serif;
           font-size: 20pt;
         }
         .tiny { font-size: 6pt; }
      </style>
      <rect x="0" y="0" width="#{table_width}" height="#{doc_height}" style="fill:black"/>
      <g transform="translate(0,0)">
        #{boxes256.join "\n"}
      </g>
      <g transform="translate(0,#{table_height*3})">
        #{boxesGrey.join "\n"}
      </g>
      <g transform="translate(0,#{table_height*3 + (h*2) + table_spacing})">
        #{boxes16.join "\n"}
      </g>
      <g transform="translate(#{w*8}, #{doc_height-(h*2)})">
        <text x="20" y="32" class="head">xterm-256-color chart</text>
        <text x="20" y="78" class="head tiny">Spooned carefully into SVG by Jason Milkins in 2012</text>
        <text x="20" y="90" class="head tiny">this SVG is in the public domain</text>
       <a xlink:href="https://gist.github.com/2868981"><text x="20" y="102" class="head tiny">https://gist.github.com/2868981 - contains the dataset in yaml</text></a>
      </g>
    </svg>
    SVG
    
    puts svg





---
### ...and here's our xterm-256color chart...

![]({{paths.media}}/xterm-256-color.svg)

