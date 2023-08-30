---
title: dqrng v0.3.1 and tikzDevice v0.12.5
author: ''
date: '2023-08-30'
slug: dqrng-v0-3-1-and-tikzdevice-v0-12-5
categories:
  - R
tags:
  - CRAN
  - package
  - R
image:
  caption: ''
  focal_point: ''
---

Today [dqrng](https://daqana.github.io/dqrng/) version 0.3.1 made it unto [CRAN](https://cran.r-project.org/package=dqrng) and is now propagating to the mirrors.

[Kyle Butts](https://github.com/kylebutts) provided the implementation for the new `dqrrademacher` method for drawing [Rademacher weights](https://en.wikipedia.org/wiki/Rademacher_distribution). The Rademacher distribution is equivalent to flipping a fair coin, which can be efficiently implementd by using the raw bit pattern from the RNG directly. See also [#50](https://github.com/daqana/dqrng/pull/50) and [#49](https://github.com/daqana/dqrng/pull/49).

Kyle also suggested a way to support random draws from a multivariate normal distribution by using code from the `mvtnorm` package, c.f. [#46](https://github.com/daqana/dqrng/issues/46). I didn't like the idea of mostly duplicating that code within `dqrng`. Fortunately, Torsten Hothorn (`mvtnorm`'s author) provided a hook in his code. So now it is possible to supply the source of normally distributed numbers from the outside, i.e. `dqrmvnorm` is just calling `mvtnorm::rmvnorm` but requests the usage of `dqrnorm`. See also [#51](https://github.com/daqana/dqrng/pull/51).

Finally, I have moved the C++ templates that are used for the fast sampling methods to their own header file `dqrng_sample.h`. This allows using them in parallel computations fixing [#26](https://github.com/daqana/dqrng/issues/26). An example is shown in the [parallel vignette](https://daqana.github.io/dqrng/articles/parallel.html#pcg-multiple-streams-with-rcppparallel).

Originally I had planned to also include support for weighted sampling in this release. This has been requested in [#18]((https://github.com/daqana/dqrng/issues/18)) and [#45]((https://github.com/daqana/dqrng/issues/45)) and I had previously had some success with [early experiments](https://stubner.me/2022/12/roulette-wheel-selection-for-dqrng/). Unfortunately the implementation based on these tests had some issues. The performance from the used probabilistic sampling gets really bad, if one (or few) possibilities have much higher weights than the others. To quantify this, one can use `max_weight / average_weight`, which is a measure for how many tries one needs before a draw is successful.

* This is 1 for un-weighted distribution or weights.
* This is (around) 2 for the random distributions used so far.
* This would be the number of elements in the extreme case where all weight is on one element.

I am not sure yet what a good cut-off point is go for a different algorithm. Or if it would be better to use the alias method right away, c.f. https://www.keithschwarz.com/darts-dice-coins/.


In addition [tikzDevice](https://daqana.github.io/tikzDevice/) version 0.12.5 made it unto [CRAN](https://cran.r-project.org/package=tikzDevice) some time ago, but I forgot to blog about it. This was a rather minor update triggered by a new `WARN` on CRAN. A recent `memoir.cls`, used for formatting the vignette, has acquired an incompatibility with the by now ancient `float.sty`. I decided to just remove the latter and rely more on LaTeX's standard methods for placing floating environments.
