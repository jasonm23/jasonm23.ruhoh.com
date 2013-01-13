---
title: Kendo mobile data-bind with formatting
date: '2013-01-13'
description: Adding custom MVVM binders to KendoUI Mobile 
categories: kendo, javascript, 

---

KendoUI's MVVM data-binding is pretty useful (as is KnockoutJS,
AngularJS, Backbone etc...

Unfortunately in Kendo's implementation `data-bind="text: property"`
binding doesn't provide a method to format property result. The
solution is quite simple though, in the form of custom binders.

As a best practice, you should create a custom binder for each the
format style you want, instead of placing a format expression as-is
into a binding attribute, this keeps your formatters in one
[DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself) place.

So, instead of trying to do: `data-bind="text: {0:c}property"` we give
outselves `data-bind="currency: property"`.

To create our new binder `currency:` we want to add a new binder
definition to `kendo.data.binders`. We know the `text:` binder does
most of the things we want, , initialising, keeping up to date,
etc. All we need to do is extend `kendo.data.binders.text` and
override its `refresh` method.

We'll create our new binder as `kendo.data.binders.currency` 

    kendo.data.binders.currency = kendo.data.binders.text.extend();
        
Then we'll replace its `refresh` method: 

    kendo.data.binders.currency.refresh = function() {
        var e = this.element;
        var p = this.bindings.currency.path;
        var s = this.bindings.currency.source;
        $(e).text(kendo.format("{0:c}", s[p]));
    };
        
Now we can use this binder in our markup.

    <span data-bind="currency: property"></span>
    
The value of `property` will be formatted to the locale currency.

