---
title: Hello World
date: '2012-06-03'
description: just a test post
categories: blurb
tags: [blurb]

layout: post
---

# Hello world

This is a test of ruhoh. Ruhoh, appears to be a rather awesome developers blogging platform.

You write your posts and pages in markdown, or some other supported style, and then use git and github (web-hook) to update the blog online.

It's pre-configured to use Bootstap the lovely web client framework from Twitter...

...Anyway, this is a test, so before I get carried away.

# Goodbye for now...

Well not quite, let's do a style test...

Python indenting with 4 spaces 

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

2 space indents are more readable?

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

