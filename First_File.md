# This is my test file.

### I created this file in tilde to test and learn different aspects of Markdown.

**This will be rendered in bold.**

*This will be in italics.*

#### What follows in an unordered list.

* Apples
* Oranges
* Grapefruit

#### This is an ordered list.

1. Preheat the oven to 350 degrees.
2. Place the cookies on the cookie sheet.
3. Allow them to bake for 5 to 7 minutes.
4. Eat them immediately and burn your mouth.

#### Below is a link to one of my favorite songs to use while playing Pathfinder.

[Bardify - Green Plains] (https://youtu.be/dyWO9NEEL9o?si=qx2AK9C0T2Tk5-rj)

#### This is some code that I cleaned up to meet the accessibility requirements for Title II.

```
<!--This is a template for an accessible accordion code as of 12-23-2025.  It should meet the WCAG 2.1 AA standard.  The default is for each accordion to contain unordered lists, but can be changed.-->

<!--This group sets the information for the bar that has the title of the accordian, and the information to open and close the accordian.  It is currently set for the first accordion to be open and all others closed.  You can change this by changing aria-expanded-"true" to "false".

    The aria-controls attribute should match the id attribute of the panel that it controls.  The data-target attribute should also match the id attribute of the panel that it controls.  The id attribute of the button should be unique for each accordion heading.  These are usually a shortened version of the title.-->

<div class="panel-group" id="accordion">
<div class="panel panel-warning">
<div class="panel-heading">
<h3 class="panel-title"><button type="button" id="heading1" aria-controls="MATCH WITH ID A" aria-expanded="true" data-parent="#accordion" data-target="#MATCH WITH ID A" data-toggle="collapse">FIRST ACCORDIAN TITLE HERE<span class="glyphicon glyphicon-collapse-down" aria-hidden="true"></span></button></h3>
</div>

<!--This group sets the information that is inside the accordion panel.  The aria-labelledby attribute links the panel to the heading that controls it.  The collapse in class makes it visible by default, and the collapse class makes it hidden by default.  The id attribute is used to link the heading to the panel. -->
<div aria-labelledby="heading1" class="panel-collapse collapse in" id="ID A" role="region">
<div class="panel-body">
<ul>
    <li>LIST ITEM</li>
    <li>LIST ITEM</li>
    <li>LIST ITEM</li>
</ul>
</div>
</div>
</div>

<div class="panel panel-warning">
<div class="panel-heading">
<h3 class="panel-title"><button type="button" id="heading2" aria-controls="MATCH WITH ID B" aria-expanded="false" class="collapsed" data-parent="#accordion" data-target="#MATCH WITH ID B" data-toggle="collapse">SECOND ACCORDIAN TITLE HERE<span class="glyphicon glyphicon-collapse-down" aria-hidden="true"></span></button></h3>
</div>

<div aria-labelledby="heading2" class="panel-collapse collapse" id="ID B" role="region">
<div class="panel-body">
<ul>
<li>LIST ITEM</li>
<li>LIST ITEM</li>
<li>LIST ITEM</li>
</ul>
</div>
</div>
</div>

<div class="panel panel-warning">
<div class="panel-heading">
<h3 class="panel-title"><button type="button" id="heading3" aria-controls="MATCH WITH ID C" aria-expanded="false" class="collapsed" data-parent="#accordion" data-target="#MATCH WITH ID C" data-toggle="collapse">THRID ACCORDIAN TITLE HERE<span class="glyphicon glyphicon-collapse-down" aria-hidden="true"></span></button></h3>
</div>

<div aria-labelledby="heading3" class="panel-collapse collapse" id="ID C" role="region">
<div class="panel-body">
<ul>
<li>LIST ITEM</li>
<li>LIST ITEM</li>
<li>LIST ITEM</li>
</ul>
</div>
</div>
</div>

<!--Add new accordions above final div tag below -->

/div>
```

## That is it for now!