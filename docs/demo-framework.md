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

Based on these criteria, I evaluated four frameworks:

- [React](#react)
- [Angular](#angular)
- [Ember](#ember)
- [Vue.js](#vue.js)

Below, I review the pros and cons of these four frameworks.

### React

[React](https://reactjs.org/) is a JavaScript library for building stateful
user interfaces. React focuses on providing a core library for building
components in pure JavaScript (with a markup extension language called JSX),
but it also comes with a large ecosystem of extensions for building full-featured
web apps, many of them (like [Redux](https://redux.js.org/)) built by core
contributors to the React project.

#### Pros

- The civic apps team uses React, and already has [well-established
  tooling](https://github.com/azavea/civic-apps-app-template) for it.

- I've been interested in learning React for some time, and I've read the
  quickstart guide already, so I'm already familiar with the basics of JSX and
  the component model.

- React is mostly pure JavaScript, with some markup added in the form of JSX.
  This is appealing to me from a developer perspective, since I don't like dealing
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

- Although many posts cite React as being simpler to learn than Angular, the
  learning curve seems like it will still be quite high, since I have to get
  acquainted with JSX syntax alongside the full conceptual framework.

### Angular

[Angular](https://angular.io/) is a web app framework written in JavaScript,
with TypeScript supported as a first-class citizen. Angular focuses on providing
a full suite of opinionated tools for building complex, stateful web apps.

#### Pros

- DRIVER is written in Angular 1.x, which has two important benefits:
    1. Many of the existing Ashlar components, like the schema editor and
       the data collector, are Angular 1.x apps.
    2. Should DRIVER want to upgrade to Ashlar 2+ during the next round of
       development, it would be easy to integrate with any work I do to
       extend the Ashlar suite using Angular (e.g. [bringing the schema editor
       up to date](https://github.com/azavea/ashlar-2018-fellowship/issues/21))

- Derek and Kathryn are already very familiar with Angular, which would make
  code review faster and more effective.

#### Cons

- As a full-featured framework, Angular has a lot of API-specific syntax
  and a complicated conceptual architecture that I'll have to learn.

- Most of the "Angular vs. React" blog posts I was able to find come out in
  favor of React, for a number of reasons (see [further
  reading](#further-reading) for links). This doesn't necessarily sway me, since
  I don't put much stock in hype, but a consistent criticism that came up in
  every piece I read was that Angular relies heavily on keeping markup and
  templating separate from JavaScript, which is a part of frontend development that
  frustrates me me. Skimming the docs, I suspect I'll have less fun learning Angular than
  I would learning other frameworks I considered (specifically React and Vue.js).

- Although TypeScript is not required to use Angular, all of the docs are
  written in TypeScript, and I would want to learn it
  for the sake of learning it anyway. While I'm interested in learning
  TypeScript, this would further increase the amount
  of time it would take me to get up to speed writing code for Angular.

- Angular purports to support native mobile applications, but I couldn't find
  any documentation on their website actually proving this. It seems like you
  have to use a third-party hybrid app framework like [Ionic](https://ionicframework.com/),
  or even [React Native](https://github.com/angular/react-native-renderer).

### Ember

[Ember](https://emberjs.com/) is a frontend framework for building web apps
using a standard model-view-controller (MVC) architecture.

#### Pros

- Ember seems to rely on traditional MVC concepts like [routers, models, and
  views](https://guides.emberjs.com/release/getting-started/core-concepts/).
  As a Django developer, I'll be comfortable with these concepts
  out of the gate.

#### Cons

- As an MVC framework, Ember represents the smallest departure from concepts
  that I'm currently familiar with. This could make development easier, but
  it also means I'm not as excited to learn it. 

- Derek and Kathryn don't have any experience with Ember, and so couldn't
  give as detailed guidance.

- Ember is [highly opinionated](https://vuejs.org/v2/guide/comparison.html#Ember),
  which could contribute to project overhead and startup costs.

### Vue.js

[Vue.js](https://vuejs.org/) is a library and a framework for building
web apps in JavaScript. Its focus is on composable views, providing a core
library for this functionality that can optionally scale up to a full framework
with batteries included.

#### Pros

- Small, both in size (~20kb zipped/minified) and in setup (can simply
  include it in a `script` tag, or set up more complicated build procedures).

- Fast learning time: the docs suggest that it takes a developer typically less
  than a day to understand the key concepts and start building ([source](https://vuejs.org/v2/guide/comparison.html#Scale)).

- Built on a standard HTML templating system, but also [supports
  JSX](https://vuejs.org/v2/guide/comparison.html#HTML-amp-CSS).

- Thorough, well-paced, and fun to read [documentation](https://vuejs.org/v2/guide/).
  Vue.js was the only framework I considered that offered [detailed
  comparisons with other similar frameworks](https://vuejs.org/v2/guide/comparison.html),
  including comparisons with React, Angular, and Ember.

#### Cons

- Native rendering is [still
  experimental](https://vuejs.org/v2/guide/comparison.html#Native-Rendering).

- Although Derek has looked into Vue.js, no one on the team has extensive Vue.js
  experience, so couldn't provide as detailed guidance.

- Similar to React, a less-opinionated framework means that it'll be more
  difficult for me to get a handle on best practices.

- Of the four frameworks I considered, Vue.js is the newest, and although it
  appears to be growing quickly its community is comparatively small.
    - For context, the StackOverflow tags for the for frameworks show:
        - Angular: ~120k questions
        - React: ~90k questions
        - Ember: ~22k questions
        - Vue: ~20k questions (~9k for Vue 2.0, released September 2016)

## Decision

Based on my review above, I recommend moving forward with **Vue.js**. To me,
Vue seems to strike the right balance between being A) a new and exciting
paradigm to learn and B) not requiring huge startup/investment costs. Even
though Derek and Kathryn don't have extensive experience with Vue, my
understanding is that Vue's focus on a simple set of core functions and low
initial investment in learning means that Kathryn and Derek could still give
feedback without having much experience, or could even familiarize themselves
with the core concepts quickly enough to understand what I'm doing in detail.

## Status

Currently in review.

## Consequences

For me, the main contest was between Vue and Angular. As such, the consequences
primarily have to do with the choice to use Vue instead of Angular. These
consequences include:

- It should be much faster for me to get started building the app itself.

- DRIVER and the Ashlar reference app won't be built on the same framework.

- Although choosing Vue for the demo app won't necessarily mean that I have to
  use it to modernize the schema editor and the data collector app, it means
  that I'm _most likely_ to do so, barring Vue being a complete disaster. This
  introduces some path dependence, and further separates the Ashlar suite
  from DRIVER development.

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

- Vue.js contributors, [Comparison with Other
  Frameworks](https://vuejs.org/v2/guide/comparison.html) (2018) 
