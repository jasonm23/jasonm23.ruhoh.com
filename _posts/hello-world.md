---
title: Hello World
date: '2012-06-03'
description: just a test post
layout: post

---

Let's do a style test...

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
            

For Haml, it's fine, and at 4 spaces it looks ugly, be that subjective or not. For CoffeeScript where lines get more complicated it's more useful to have 4 space indents for the same reason as Python. 

This could happen in Haml too, although if you're using Haml properly it definitely shouldn't.

