---
layout: post
title:  "Flight and Rails - Part I"
date:   2014-05-19 12:30:14
categories: rails
author: siddhartha
---

### What is flight?
Flight is an evented client side JavaScript framework from Twitter.

### What does it do?
As the amount of client side JavaScript grows, the code inevitably starts becoming hard to manage and complicated (probably duplicated), despite all your code being organized into functions. Flight helps clean this clutter up by making the code decoupled, modular and evented, thus making the code easy to manage and even debug. Lets look at an example.

### Regular JavaScript Example
<p data-height="150" data-theme-id="0" data-slug-hash="xIfKg" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/siddharthabhagwan/pen/xIfKg/'>Basic Add Subtract</a> by Siddhartha Bhagwan (<a href='http://codepen.io/siddharthabhagwan'>@siddharthabhagwan</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

This is a small program that increments and decrements a value on a button click.

### The Flight approach

<p data-height="268" data-theme-id="0" data-slug-hash="EnlIG" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/siddharthabhagwan/pen/EnlIG/'>Flight Add Subtract</a> by Siddhartha Bhagwan (<a href='http://codepen.io/siddharthabhagwan'>@siddharthabhagwan</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

Flight consists of standalone componenets, that encourages decoupling. A component can emit a trigger, and a listen for such triggers and choose to react to one or more triggers.

#### What is a component?
A component is a UI element that can undergo a change/transition or have an event generated on it. Examples include a button that is clicked, or a drop down list whose value has been changed/set, or a text box that has been enabled or disabled.


#### The Add Button Component

    var add_component = flight.component(function() {

      this.add_1 = function(event) {
        this.trigger('addButtonClicked');
      };
      
      // Component listeners
      this.after('initialize', function() {
        this.on('click', this.add_1);
      });
    });

    add_component.attachTo('#add_button');

The `add_component` is attached to `#add_button`, thus providing a context i.e, reference to `this`. The component listeners block, set after the component is initialized, decides what all will the component react to, and how ill it do so. `add_component` will call the `add_1` function when it gets clicked. The `add_1` function in turn emmits a trigger `addButtonClicked` , which shall be available to all other components throughout the document. The responsibility of the `add_1` fcuntion ends here.

#### The Subtract Button Component

    var subtract_component = flight.component(function() {

      this.sub_1 = function(event) {
         this.trigger('subButtonClicked');
      };
      
      // Component listeners
      this.after('initialize', function() {
        this.on('click', this.sub_1);
      });
    });

    subtract_component.attachTo('#sub_button');

The `subtract_component`is very similar to the `add_component` in behaviour and functionality

#### The Result Box Component

  var result_box_component = flight.component(function() {

      this.add = function(event) {
        current_val = $('#result_box').val();
        $('#result_box').val(parseInt(current_val) + 1);
      };
      
      this.sub = function(event) {
        current_val = $('#result_box').val();
        $('#result_box').val(parseInt(current_val) - 1);
      };
      
      // Component Listeners   
      this.after('initialize', function() {
        this.on(document, 'addButtonClicked', this.add);
        this.on(document, 'subButtonClicked', this.sub);
      });
    });

    result_box_component.attachTo('#result_box');

The `result_box_component` listens for two events i.e, `addButtonClicked` and `subButtonClicked` and calls the `add` function and the `sub` functions on these events respectively


#### The flight code base is significantly larger than the plain jQuery code for even such a small functionality
Agreed. The boilerplate code for flight may seem unnecessary at first, but as you add more functioanlity, the flight code base does not increase exponentially. Infact, there are a few things worth noticing here -

- No component treads into the space of the other. Unlike the jQuery code, where clicking the `add_button` modifies the `result_box`, here the `add_box_component` only emmits a trigger. All the manipulation with respect to the result box is done within the `result_box_component` itself. So the complete functionality of a component is now visible at one place.

- For any new funtionality that needs to be added to the result box, we just have to add a listener, and a function.

