# ADR 2: Choosing a new name for the package

Jean Cochrane (Open Source Fellow, Summer 2018)

## Context

The name Ashlar [is already taken on PyPi](https://pypi.org/project/ashlar/).
If we want to distribute our package on PyPi, we'll have to either:

1. Convince the owners of `ashlar` to give it to us
2. Come up with a new name for the project

Option 1 seems unlikely, given the maturity of the ashlar package on PyPi and
how recent the last release was (April 2018, less than six months ago). Instead,
I believe that we must choose option 2 and rename the project.

## Decision

I propose that we rename the project to **Silicone**.

Silicone is a material widely known for A) its physical flexibility and B) its
practical versatility. The [Wikipedia page for
silicone](https://en.wikipedia.org/wiki/Silicone) lists a wide variety of
different products made with silicone, ranging from consumer products to
construction materials.

"Silicone" respects the origins of Ashlar by repurposing a construction material
for the name of the package, but emphasizes the core feature of the project --
its flexibility and versatility as a material with which more complex products
can be built.

The "Silicone suite" (as a reference to the multiple tools that can be used
with the library) is a nice alliteration, as well.

Perhaps most importantly, `silicone` is available on PyPi, and [I'm currently
squatting on it](https://pypi.org/project/silicone/).

## Status

Currently drafting the ADR. After that, this will be in review.

## Consequences

After changing the name, we'll have to:

- Create a new repo for the project
- Fork Ashlar and move the code over to Silicone
- Update the code to refer to `silicone` instead of `ashlar` internally in
  all imports and namespaces
- Rename repos that reference Ashlar, including;
    - `ashlar-2018-fellowship`
    - `ashlar-blueprint`
