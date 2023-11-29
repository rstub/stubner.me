---
title: Bug fixes for dqrng and tikzDevice
author: ''
date: '2023-11-29'
slug: bug-fixes-for-dqrng-and-tikzdevice
categories:
  - R
tags:
  - CRAN
  - R
  - package
image:
  caption: ''
  focal_point: ''
---

Today [dqrng](https://daqana.github.io/dqrng/) version 0.3.2 made it unto [CRAN](https://cran.r-project.org/package=dqrng) and is now propagating to the mirrors. In addition [tikzDevice](https://daqana.github.io/tikzDevice/) version 0.12.6 made it unto [CRAN](https://cran.r-project.org/package=tikzDevice) as well. Both releases are minor bug fixes triggered by a new `WARN` on CRAN. Recently the development version of R has started to take compiler warnings with respect to format errors more seriously, forcing a large number of CRAN maintainers to become active. In my case things where rather simple: For `dqrng` I only needed to recreate `RcppExports.cpp` with the latest `Rcpp` version, c.f. https://github.com/RcppCore/Rcpp/issues/1287#issuecomment-1829886024, while `tikzDevice` needed a minor fix in one error message.
