+++
title = "Make your application actually adaptable"
date = "2019-05-14T01:39:31Z"
tags = ["javascript","presentation"]
draft = true
author = "admin"
+++

Preparing draft for presentation on Frontend Reunited

Hi I am manish Jung thapa, I will be sharing my experience on this topic, adopting with change: Frontend Architecture and patterns

## My History of javascript

As a user My early experience was when Javascript used to be primitive . Image slider was the only cool thing. Password validation.
There was a change in behavior, checking Username or email already exists or not was done without refreshing a page, AJAX .
Web is evolving.
whole web ecosytem took a leap, now its more like web app than just websites. It grew larger larger on both capabilities and complexities.
Different concepts and tools arising to solve different problems and complexites.

## JS Modules

A lot of old patterns fall away with new modules on ES 6
Two patterns here in this code snippet

- Immedietly invoked function expression
- wrapped inside so that nothing escapes accidentally to window or global scope
- use of string literal "USE Strict" makes javascript into the strict mode which can reduce the number of strange errors/strange behavior that js brings and sometimes even leads to performance boost

Es 6

- No more IFFEs
- any file with .js is now a module
- nothing goes to global scope unless we explicitly try to put it there by going to the widow object
- not required use strict declaration anymore because modules are by default interpreted and executed as in strict mode

using export
We can use `export` to export varibles, functions, class and declare default from the module

Similarly we can use `import` to import module and import variables, functions, class from modules

Also Import creates immutable bindings

## Components

Concept of components

- One component should focus on one section of the UI.
- One component shouldn’t access another’s html directly.
- The component encapsulates its Output(html), Style (css) and Functionaly (javascript).
- Each component has some way to interact with others.

This approach has many advantages. It aligns with Single Responsibility Principle, so we have good separation of concerns, which in turn makes debugging easier.

Due to these advantages every major frameworklike React, Vue, Angular uses this.

lets talk about reusable components, lets create a table and call it Datatablex than can be reuse

Feature of data table

- sortable
- index
- filter
- pagination

## Structuring the modules

#### structuring the files

Sturcturing the modules in different folders and sub folders plays vital role in understandability and shows objective of module at glace. We can aline structuring files with concept of MV\* pattern. Keypoint I take considering while stucturing are, separation of concerns and feature based.... like, service layers, routes/views, components, etc and each component has its own functionaly and is self contained

## Building Modules

Strange situation with javascript is, ES 6 defined modules and syntax is awesome but there's not a single environemnt out there that natively does module loading, so theres no browsers widely used that natively supports import statements....
Therefore we have to use one of the build tools that analyse the dependencies, go through source code, to import statements and follow them and create bundles into single or multiple files

Toolchain can impact performance

Making webpack to build for development and production can help developers. like while developing, we can allow logs to show and in production to have all codes minified and console logs removed, we can set different api paths

## CSS / SCSS

Even not being a better desinger, a frontend developer can't be out of css. Lets discuss a little about css and scss.

-making a class with specific properties. Like btn class can have a property, in general buttons must have, and btn-warning can have specific property that resembles its nature

Scss is better tool for frontenders. Best thing I like about it is variables. Say we have a product and uses a primary color red and its variance everywhere like in background it uses 50% of red, in shadow 80% of red, and highlighted with say 20% of red. so in css when we have to change it, we have to go through each and change it, or search and replace all. but with scss variable, we can change it at once and reuse everywhere

Another amazing feature of scss is Mixins, its functions for css.
