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
before, so the startup costs for development will be high and the work invested
will not be useful unless we continue development with the same framework. In
other words, the costs of picking the wrong framework are potentially quite
high.

In evaluating frameworks, we're looking for one that will be:

1. Easy for me to pick up in a couple weeks
2. Familiar enough to Derek and Kathryn that they can provide code review and
   guidance on architecting the app
3. Exciting for me to learn to use

Below, I review a few possibilities I considered based on these criteria.

### React

#### Pros

- The civic apps team uses React, and already has [well-established
  tooling](https://github.com/azavea/civic-apps-app-template) for it.

- I've been interested in learning it for some time.

- React is mostly pure JavaScript, with some markup added to JSX. This is nice,
  since I hate dealing with HTML.

- Well-supported hybrid mobile SDK in React Native, should I want to port the
  app to mobile someday.


#### Cons

- React is more of a library than a framework, and is not very opinionated about
  how to handle meat and potatoes things like AJAX calls
  ([source](https://medium.freecodecamp.org/angular-2-versus-react-there-will-be-blood-66595faafd51#c87b).
  This means that it may be harder for me to figure out my dependencies and
  get a handle on best practices quickly.

### Angular

[Angular](https://angular.io/) is a JavaScript app framework developed by Google.
It has both [web]() and [mobile]() SDKs.

#### Pros

- DRIVER is written in Angular.

- Derek and Kathryn are both very familiar with Angular.

#### Cons

- Angular purports to support native mobile applications, but I couldn't find
  any documentation on their website actually proving this. It seems like you
  have to use a third-party hybrid app framework like [Ionic](https://ionicframework.com/),
  or [React Native](https://github.com/angular/react-native-renderer).

- As a full-featured framework, Angular has a lot of framework-specific syntax
  that I'll have to learn.

- Would require me to learn TypeScript to use the most recent versions.

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

Some good resources I used while evaluating frameworks:

- Cory House, [Angular 2 versus React](https://medium.freecodecamp.org/angular-2-versus-react-there-will-be-blood-66595faafd51#c87b)
  (2016)

- Dominik Tarnowski, [Angular vs React](https://hackernoon.com/angular-vs-react-the-deal-breaker-7d76c04496bc)
