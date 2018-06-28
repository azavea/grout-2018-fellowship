# ADR 1: Choosing a framework for the demo app

Jean Cochrane (Open Source Fellow, Summer 2018)

## Context

As part of the [Ashlar 2018 summer fellowship](https://github.com/azavea/ashlar-2018-fellowship),
we want to build a demo app on top of the Ashlar suite in order to:

1. Provide a compelling proof-of-concept for the Ashlar suite
2. Create more reference code (in addition to the [reference
   implementation](https://github.com/azavea/ashlar-blueprint)) to help
   future users understand how to build on top of Ashlar
3. Help guide the development of the Ashlar suite to ensure that we prioritize
   the most impactful features

An important component of this development, however, involves selecting
a frontend framework to use to build the demo app. This particular decision is
important because I've never built an app from scratch using a frontend framework
before, so the overhead for development will be high and the work invested
will not be useful unless we continue development with the same framework. In
other words, the costs of picking the wrong framework are potentially quite
high.

In evaluating frameworks, we're looking for one that will be:

1. Easy for me to pick up and build a simple app in 2-4 weeks
2. Familiar enough to Derek and Kathryn that they can provide code review and
   guidance on architecting the app
3. Exciting for me to learn to use

Below, I review a few possibilities I considered based on these criteria.

### React

[React](https://reactjs.org/) is a JavaScript library for building stateful
user interfaces.

#### Pros

- The civic apps team uses React, and already has [well-established
  tooling](https://github.com/azavea/civic-apps-app-template) for it.

- I've been interested in learning React for some time, and I've read the
  quickstart guide already, so I'm already familiar with the basics of JSX and
  the component model.

- React is mostly pure JavaScript, with some markup added in the form of JSX.
  This is appealing to me from a developer perspective, since I hate dealing
  with templates and I want more experience writing complex JavaScript.

- Although it has the same problems as many hybrid mobile frameworks, React Native
  is a comparatively well-supported library for porting React apps to mobile,
  which would seem to open up the possibility of potentially creating a native
  version of the demo app sometime in the future.

#### Cons

- React is more of a library than a framework, and is not very opinionated about
  how to handle meat and potatoes things like AJAX calls
  ([source](https://medium.freecodecamp.org/angular-2-versus-react-there-will-be-blood-66595faafd51#c87b).
  This means that it may be harder for me to get a handle on best practices quickly.

- Related to the point above, React will likely have a less clear "path" to
  building the kind of app I want to build, which will incur more time
  researching appropriate packages.

- Derek and Kathryn may not have as much experience in React as in Angular (?).

### Angular

[Angular](https://angular.io/) is a web app framework written in JavaScript,
with TypeScript supported as a first-class citizen. 

#### Pros

- DRIVER is written in Angular 1.x, which has two important benefits:
    1. Many of the existing Ashlar components, like the schema editor and
       the data collector, are Angular 1.x apps
    2. Should DRIVER want to upgrade to Ashlar 2.0 during the next round of
       development, it would be easy to interoperate with any work I do to
       extend the Ashlar suite using Angular (e.g. [bringing the schema editor
       up to date](https://github.com/azavea/ashlar-2018-fellowship/issues/21))

- Derek and Kathryn are already very familiar with Angular, which would make
  code review faster and more effective.

#### Cons

- As a full-featured framework, Angular has a lot of framework-specific syntax
  that I'll have to learn.

- Most of the "Angular vs. React" blog posts I was able to find come out in
  favor of React, for a number of reasons (see [further
  reading](#further-reading) for links). This doesn't necessarily sway me, since
  I don't generally put much stock in JS-framework hype. However, one consistent
  criticism that came up in every piece I read was that Angular relies heavily
  on markup and templating, which is the part of frontend development that is
  least appealing to me. I suspect I'll have less fun learning Angular than
  I would learning React.

- Although TypeScript is not required to use Angular, I would want to learn it
  for the sake of learning it anyway. This would further increase the amount
  of time it would take me to get up to speed writing Angular.

- Angular purports to support native mobile applications, but I couldn't find
  any documentation on their website actually proving this. It seems like you
  have to use a third-party hybrid app framework like [Ionic](https://ionicframework.com/),
  or even [React Native](https://github.com/angular/react-native-renderer).


### Elm

[Elm](http://elm-lang.org/) is a purely functional language for building
web apps that compiles to JavaScript.

#### Pros

- The Geospatial Insights team is starting to use Elm in production; using it
  for the Ashlar demo app could be a good way of testing the limits of its
  usefulness in a relatively low-stakes context.

- Elm is a purely functional language, which aligns with my goal of continuing
  to learn more about functional programming. Of the three options I considered
  (React, Angular, and Elm), I'm personally most interested in learning more
  about Elm.

#### Cons

- Elm involves a whole language with its own syntax in addition to frameworks/
  libraries for common web app functions. This will greatly increase the
  overhead costs for learning.

- The Elm community is much smaller than the JS community, and in general Elm
  [makes it intentionally difficult to interact with JS
  libraries](https://guide.elm-lang.org/interop/javascript.html#step-2-talk-to-javascript).
  This means that in Elm, we'll have much less flexibility to leverage existing
  open source code to build the app.

- Since Elm does not easily interoperate with JS, we'll need to either forego
  the [Ashlar JavaScript API](https://github.com/azavea/ashlar-2018-fellowship/issues/20),
  write it in Elm instead, or write Elm bindings for it.

## Decision

Based on my review above, I recommend moving forward with **React**.

## Status

Currently in review.

## Consequences

## Further reading

Some references I used while evaluating frameworks:

- Cory House, [Angular 2 versus
  React](https://medium.freecodecamp.org/angular-2-versus-react-there-will-be-blood-66595faafd51)
  (January 2016)

- Dominik Tarnowski, [Angular vs
  React](https://hackernoon.com/angular-vs-react-the-deal-breaker-7d76c04496bc)
  (January 2017)

- Krusche Company, [Angular 6 vs React
  16.3](https://kruschecompany.com/blog/post/angular-6versus-react-16.3)
  (May 2018)

- Elm contributors, [An Introduction to Elm: Javascript
  interop](https://guide.elm-lang.org/interop/javascript.htm) 
