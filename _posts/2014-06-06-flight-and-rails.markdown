---
layout: post
title:  "Flight and Rails - Part I"
date:   2014-05-19 12:30:14
categories: rails
author: siddhartha
---

### What is it and why do I care?

Off late, with the potential of client side javaScript being explored, theres been an explosion of client side JS frameworks. FlightJS , from the creators of Twitter. You should totally chuck all your other frameworks and switch to FlightJs becase < insert bold omnipotent claim here >

Flight may or may not be a replacement for your current JS framework, in case you have any. Its more about the right tool for the right job. Flight is an evented framework for cleaner and modular front end code. It consists of standalone componenets, that encourages decoupling. A component can emit a trigger, and a component can listen for such triggers, and decide to act upon one or more triggers. Having a set of components makes the code quite easy to manage and even easier to debug.

Lets get into an example to understand this better. Below is some html and its corresponding javascript, for a simple tab and pane using bootstrap. Please note that there are two ways to go about this, i.e, using flight with RequireJs, or using standalone flight. This blog goes with the latter.

<p data-height="100" data-theme-id="0" data-slug-hash="hzIAC" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/siddharthabhagwan/pen/hzIAC/'>hzIAC</a> by Siddhartha Bhagwan (<a href='http://codepen.io/siddharthabhagwan'>@siddharthabhagwan</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

In the flight world of doing this, there would be a slightly different approach to this.

<p data-height="100" data-theme-id="0" data-slug-hash="yrBpv" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/siddharthabhagwan/pen/yrBpv/'>yrBpv</a> by Siddhartha Bhagwan (<a href='http://codepen.io/siddharthabhagwan'>@siddharthabhagwan</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

####Flight Components
Flight-ed code consists of components. A component corresponds directly to an element visible on your page that can/does undergo some change/transformation. Examples could be a button, whose color changes, or that gets enabled or disabled based on certain conditions. It could be a drop down list, whose value is changed, or gets set to something.

In our case, we have 2 components - the tab, and the tab-pane. One might ask why isn't the tab-pane a singular structure, or, if they are indeed two different components, why isn't one a tab and the other a pane? In this case, thats just the way [Bootstrap Tabs](http://getbootstrap.com/javascript/#tabs) work.

As mentioned earlier, a components emmits triggers, and also listens for them, and reacts accordingly. The 1st component listens for a click on the tab. When this happens, it emmits a trigger `uiToggleTab`, together with the id of the clicked element. Try to understand this as the Tab yelling out - "I have been clicked! and here's my id (so you know what 'I' means or who 'I' is)".

Before preceeding, its important to note the last 2 lines of the Flight code - 

    tab_component.attachTo('.js-tab > a');
    tab_pane_component.attachTo('.js-tabpane');

The components are attached to certain elements in the document. There are 2 things to be noted about this. To detect a click on Tab A and on Tab B, there is no repetition or duplication of code as the `tab_component` is not attached separately to the two tabs using any id. Rather the component is attached to the `js-tab` class common to all the tabs. The `tab_component`, when invoked, fetches the id of the clicked tab by traversing the `$node` object - `this.$node.parent().data('tpid')`. The other thing to note is that the component is attached to (in case of tab_component) `'.js -tab'`. The code will work even if it is attached to 'document', but then the component will listen to for `uiToggleTab`, throughout the document. Here, this is unnecessary, because we know for sure, that the triggers are going to be generated only and only on clicks to the link elements with the `.js-tab` class, and so we attach it specifically to that.

_In a manner of speaking, this is a Pub-Sub implementation_

The second component listens on the `uiToggletTab` trigger i.e, it listens for a tab being clicked. Like the tab component, the tab_pane_component is attached to multiple elements via `.js-tabpane`. Here, both the tabs and both the panes i.e, four elements listen and react the `uiToggleTab` trigger. Each element checks the argument that has been passed along with the trigger, and matches it to the elements own id. If it matches, the element shows itself by adding the `active` class to itself. If not, it removes the `active` class from itself.

####The Cool Part
Addition of more tabs and panes now requires **no** additional JavaScripting. Just add the html with the appropriate classes, i.e, `js-tab` for the tab and `js-tab-pane` for the tab pane, and we're done. You can try this by editing the html of the second code snippet.

####Cool. So every function can now be a component and receive and emmit triggers!
No. Flight isn't aimed at converting any and every function into a component. Chances are, there **will** be a genric function thats used by multiple functions/components, that doesn't need to be converted in a component. Only UI elements that have a certain functionality need to be componentized. Similarly, non UI functions that deal with reading/writing of data, can be made into Flight components.

