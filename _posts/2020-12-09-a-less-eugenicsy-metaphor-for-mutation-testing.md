---
title: "A Less Eugenics-y Metaphor for Mutation Testing"
publish: true
---

Lately I've been hanging out in the mutation testing Discord which was set up by
Markus (author of the Ruby gem [`mutant`][mutant]). The Discord is an attempt to
bring together the authors (as well as super-fans like me!) of mutation testing
tools for different languages for mutual support.

One of the problems that's been identified is that the entire concept of
mutation testing is tough to grasp from nothing and that the different tools can
refer to the same concepts with different language (is a mutation a change, or
the codebase after the change?)

Well, I'm here to make that problem *worse*!

One of the things that leaves a slightly sour taste in my mouth is the wording
used around "killing" mutants / mutations. Your tests "kill" a mutant/mutation
when they fail, indicating that they cover the semantics of the original code
which got altered.
But "killing mutants" sounds a little bit violent, and if you really think
about it, a bit eugenics-y. It can also sound a bit negative (do you want
something related to your code "killed"?) and a bit jargon-y.

I'd like to stimulate a discussion around different terminology we could use -
perhaps a metaphor borrowed from a different field, or perhaps we could bring
the wording closer to the specific meaning of the testing being in the code
domain. I do this in full knowledge that nothing is likely to change - or if it
is it will be on a glacial pace - it would most likely be damaging to the
mutation testing community to alter the terminology, and certainly to any
individual currently existing project.

With all that in mind, here's a not-so-serious attempt at a musical mutation
testing metaphor:

* **Composer** - A team of software developers
* **Work** - A software project
* **Understudy** - The software project's test suite
* **Conductor** - A mutation testing tool

* You are a **Composer** writing a **Work** - The Work is your application.
* You have an **Understudy** who you have been teaching the nuances of your
  Work.
  * The Understudy can tell when something's wrong with the Work, based on the
    knowledge you've given them about the Work.
* The orchestra (the computer) are all robots and can perform the Work exactly
  as it is written.
  * They rehearse with the Understudy present (i.e. execute the test suite)
* Each rehearsal the **Conductor** starts from your original written Work and
  makes one change - maybe removing a note, or changing the articulation, or
  removing a measure from the Work.
  * If the Understudy notices that there's something wrong with the Work, that's
    that's good - they obviously know enough about the Work to figure out what
    the Composer changed.
  * If not, then either the Understudy doesn't know enough about the Work, or
    the Work could be simplified by making the change the Conductor did.

After creating this metaphor I then had a bit of a lightbulb moment, realising
that [Generative Adversarial Networks][GANs] are kinda like
self-mutation-testing neural networks. Perhaps there's some less-violent
terminology we can poach from the AI sphere for mutation testing.

[mutant]: https://github.com/mbj/mutant
[GANs]: https://en.wikipedia.org/wiki/Generative_adversarial_network
