---
title: ...and we're back
date: '2012-06-03'
description: just a test post
layout: post

---

Ok this isn't a real post, and it's not an introduction, if you pass by here, I'm sure it's not because of my welcome post or an about me, it's because googling for some help has landed you here, in which case I hope coming here helped.

While we're at it let's do a style test...

Python indenting with 4 spaces is good...

    def some_callable(argument1, argument2=False):
        """ Doc-string for some_callable function. """
        if not argument1:
            return None
        try:
            argument1.do_stuff()
        except (AttributeError): # some comments
            if not can_fail:
                raise
        return argument1 

Indenting 2 spaces is less readable.

    def some_callable(argument1, argument2=False):
      """ Doc-string for some_callable function. """
      if not argument1:
        return None
      try:
        argument1.do_stuff()
      except (AttributeError): # some comments
        if not can_fail:
          raise
      return argument1 

However, in Ruby it's more readable to use 2...

    def self.create_with_omniauth(auth)
      create! do |account|
        account.provider = auth["provider"]
        account.uid      = auth["uid"]
        account.name     = auth["info"]["first_name"]
        account.surname  = auth["info"]["last_name"]
        account.email    = auth["info"]["email"
        account.role     = "users"
      end
    end

Interestingly, Haml, CoffeeScript and Sass all suggest a 2 space indent, even though they share similarities with Python due to their whitespace sensitive syntaxes.

    .navbar
      .navbar-inner
        .container
          %a.brand(href="/") #{title}
          %ul.nav
            - pages.each do |p|
              %li= partial :page, :locals => {:p => p}
            

Anyway, enough of that, let's have a look at markdown support ... 

# Heading h1

## Heading h2

### Heading h3

#### Heading h4

##### Heading h5

###### Heading h6

Ordered lists....

1. List
  1. List
  1. Nesting
1. Ordered
1. Items
1. And 
  continuation?
  
1. Resume...

Unordered Lists

* This unordered list
  * isn't too bad,
    continuation can be problematic with lists though
    we'll see eh...
    
  * and resume...
* Rule of thumb with markdown, don't try and be too clever with it.
* and 
* it'll serve you well.

> Blockquoting support is simple too.
> of course, we have `in line code` and
> **bold** *emphasis/italics*.

Horizontal rules are allowed with just a flick of the `---`

--- 

Mind you, with Bootstrap they can be hard to spot 

> You can also use <sup>superscript</sup> in 
> some markdown parsers, it depends on their **&lt;HTML TAG&gt;** support.

...and that about wraps it up, Ruhoh, a really straight-forward and BS free blogging experience. 

Thank you very much Jade, it's awesome.
