---
title: Element Hierarchy - Webflow
subtitle: "Source: Webflow"
category:
  - Webflow
author: Noder
date: 2020-05-27T07:02:29.189Z
featureImage: /uploads/maxresdefault-4-.jpg
---
## Lesson info

To succeed in web design, and with Webflow, it's vital that you understand how elements hierarchy affects styling and sizing. In this video, we’ll cover element hierarchy in three ways:

1. ‍How objects nest inside one another (e.g., parents, children, siblings)
2. ‍How text styling is passed down from parent to child
3. ‍How the sizes of parent elements and child elements affect one another

[![Watch the video](https://i.ibb.co/f4nVsKR/Start-tutorial-perfect.png)](https://youtube.com/embed/SffN3U1qOl0)



## Transcript

Hierarchy is one of those funny words which came from the Germans. Literally translated from 15th-century text, it means: “website levels.”

In our Webflow projects, we’ll talk about hierarchy in three ways:

1. How objects nest inside of one another and become parents, children, siblings.

2. How text styling passes down through this hierarchy.

…and 3. How the size of parent elements and child elements can affect one another.

Let’s start with nesting.

Nesting is simply placing one object inside another. So if we have a section in our project and inside that we add a Heading, that Heading has now been placed or nested inside the section. We can see this really clearly over in the Navigator. We have the section…and we have the Heading.

Now our heading is a child element of the section. And if we add a paragraph in here? Maybe a button? They’re now children of the Section element, too. They’re siblings.

So what’s our hierarchy here? At the top level, we'll click Body, and our view will slide up to the top of the page. So that’s our top-level element.

And if we look immediately under that, all its child elements are slightly indented. That horizontal indentation indicates that they’re children (child elements of the Body). So to elements on this level, their parent element is the Body.

Now what about our new section? Well, we know it’s a child of the Body, but it’s also a parent element to the content we put inside. And that content is indented even more. That horizontal indentation indicates that these elements are children of our Section.

The second visual representation of this hierarchy, is accessible underneath the Canvas: in the bottom navigation. We can select any element, and quickly determine its relationship to its parent elements…all the way up to the Body.

Now it won’t show us sibling elements, for instance if we click on our new heading, it’s not going to show the paragraph or the button, but it will show its direct parent. And its direct parent. And just like in the Navigator, these elements are fully clickable, and we can access anything in that hierarchy.

That's nesting.

Up next is text styling. Not to be confused with textiling or tech styling.

So we’ve selected this page’s body. And if we start playing with some of our typography options, you’ll see they’ll pass right down. Now only typography styles behave this way: changes to typography styles on a parent element can affect its children, and in this case, its children's children. And that can go as deep as you want.

Now children can override these styles. For instance, we can select the section and go into the typography, and make some changes here. Notice how these changes are overriding the changes we made to our body. Children can override typography styling set by their parent. Let’s go deeper. Select our heading, adjust the typography on that. Same deal. Children can override these styles.

And the Style Panel gives us the most helpful tool ever when we’re trying to determine what’s going on. If we select an element and it’s inheriting a style? We can just press on the indicator and find out exactly where it’s getting that style.

That's hierarchy as it relates to text styling.

Finally, we have size.

Most elements are sized by the content inside them. For our section here, it has no defined value. And adding more content makes the section taller!

Parent elements can override the natural document flow, though — this can happen if we set fixed values. If we define a height, for instance. Or even a width.

Children affect the size of their parents; but giving a parent a set size can override the natural document flow.

So. Three aspects of element hierarchy we deal with in our projects:

We got nesting, where parent elements can contain other elements — child elements. We can see this in the Navigator, and on the bottom navigation under the Canvas.

We have text styling: passing down styling from a parent object to text in its children, or its children’s children.

And of course sizing. The size of a parent element can be determined by what’s inside. The parent’s child elements determine the size of the parent…unless we override that size on the parent.

And that…is element hierarchy as it relates to our projects.
