# ADR 2: Choosing a new name for Ashlar 

Jean Cochrane (Open Source Fellow, Summer 2018)

## Context

The name `ashlar` [is already taken on PyPi](https://pypi.org/project/ashlar/).
Since PyPi requires unique names for packages, this means that if we want to
distribute our package on PyPi, we'll have to either:

1. Convince the owners of `ashlar` to give it to us
2. Come up with a new name for the project

Option 1 seems unlikely, given the maturity of the ashlar package on PyPi and
how recent the last release was (April 2018, less than six months ago). Instead,
I believe that the best course of action is to choose option 2 and rename the project.
This will require us to come up with a new name for Ashlar, a [notoriously
difficult decision](https://martinfowler.com/bliki/TwoHardThings.html).

## Decision

I propose that we rename the project to **Silicone**.

Silicone is a material widely known for its physical flexibility and its
practical versatility. The [Wikipedia page for
silicone](https://en.wikipedia.org/wiki/Silicone) lists a wide variety of
different uses for silicone, ranging from consumer products to
construction materials.

The name "Silicone" respects the origins of Ashlar by making reference to a
construction material, but unlike "Ashlar", the name "Silicone" emphasizes the
core features of the project -- its flexibility and versatility as a base material
with which more complex products can be built.

As an added bonus, "the Silicone suite" (as a reference to the multiple tools
that can be used with the library) is a nice alliteration, as well.

Perhaps most importantly, `silicone` is available on PyPi, and [I'm currently
squatting on it](https://pypi.org/project/silicone/).

## Status

Currently drafting the ADR. After that, this will be in review.

## Consequences

- After changing the name, we'll have to take a number of steps to update the
  project:
    - Create a new repo for the project
    - Fork Ashlar and move the code over to Silicone
    - Update the code to refer to `silicone` instead of `ashlar` internally in
    all imports and namespaces
    - Rename repos that reference the name "Ashlar", including;
        - `ashlar-2018-fellowship`
        - `ashlar-blueprint`

- Since "Ashlar" is a more esoteric name than "Silicone", the package may lose SEO.

- We'll be able to distribute the project on PyPi with a short and memorable
  name.
