# GSoC 2022 Final Report: Building `Survey.jl`

**Contributor:** Iulia Dumitru ([iuliadmtru](https://github.com/iuliadmtru))  
**Project URL**: https://github.com/xKDR/Survey.jl  
**GSoC Project Page:** https://summerofcode.withgoogle.com/programs/2022/projects/HZt97ZtA

With this project, I embarked on the mission of porting Thomas Lumley's `survey` package
from R to Julia, or at least the basic structure of it. The initial plan was to implement
as much functionality as possible to have a useful, rudimentary package. When I
started the project, work on the package had already been started by the people at
[xKDR](https://github.com/xKDR). This was the version that was there when I started:
[commit 8791c50](https://github.com/xKDR/Survey.jl/commit/8791c50e93d775015df100b84fa422cf5ad0d0f9).
From the functionality I mentioned in my proposal, these functions and types were already
implemented:

— `svydesign` — type and constructor

— `svyby` — function

— `svyglm` — type and constructor

— `svymean` — function


But none of these were complete, except maybe for `svyby`. So, there was a lot of work to
do to reach the present state. I will talk about each step in my journey.

## First contact

The first 2-3 weeks of the program were by far the hardest. During this time, I almost
exclusively read documentation ([Julia](https://docs.julialang.org/en/v1/) and [R, survey](https://cran.r-project.org/web/packages/survey/survey.pdf)),
Discourse posts (about [DataFrames](https://discourse.julialang.org/t/replace-values-in-a-dataframes-column-with-another-value/79548)
and [groupby](https://discourse.julialang.org/t/groupby-for-regular-array/9877)) and
forums (mostly [StackOverflow](https://stackoverflow.com/questions/50038413/fill-bubbles-with-color-in-svyplot-in-r-survey-package)). My first PR wasn't until
[July 11](https://github.com/xKDR/Survey.jl/commit/ab0908c7c502bfa54cb1e031bec7f83ecdf73f0c),
even though no time was wasted. Anyway, there's not much to talk about here, so let's skip
straight to the interesting part.

## Excitement

This is when I basically became addicted. I started committing like crazy, to the point
where I became the second contributor of the package. To be fair, there are not many
contributors at this point since it is a new package but still, I consider it a big deal.
During this time, we implemented most of the functionality that is now in the package:

— complete `svydesign`

— `svymean`, with standard errors

— `svytotal`

— `dimnames`

— `svyplot`

— `svyhist`

— `svyboxplot`

Maybe it doesn't sound like much, but there were sweat and tears pouring out for these
functions. Especially with `svymean` because we just couldn't get the standard error
correct.

Meanwhile, we were getting more attention from other contributors. Issues started appearing
([#4](https://github.com/xKDR/Survey.jl/issues/4), [#27](https://github.com/xKDR/Survey.jl/issues/27)),
mostly related to the style of the package. People were worried that going for an R-like API
was not the best idea. So, we started discussing what to do next: should we keep
working on the version we have now, or should we start from scratch with a new style?
We decided to [ask for the community's opinion on Discourse](https://discourse.julialang.org/t/suggestions-for-the-design-of-survey-jl/86381)
and we realised it was best to start from scratch.

## Falling into the Chasm of the Unknown
At this point, I needed another break from coding to dive into research. I read even more
documentation ([Constructors](https://docs.julialang.org/en/v1/manual/constructors/)),
even more Discourse posts (about [keyword argument constructors](https://discourse.julialang.org/t/automatic-keyword-argument-constructor/36573) or
[keyword arguments in anonymous functions](https://discourse.julialang.org/t/extracting-kwargs-from-anonymous-function/37350)),
the [Julia style guide](https://docs.julialang.org/en/v1/manual/style-guide/),
[everything](https://en.wikipedia.org/wiki/Type_theory). If we were
starting fresh, we needed to start _perfectly_. Obviously, this wasn't a good approach
and I ended up procrastinating and ordering books like _Man's Search for Meaning_ or 
_Meditations_. At some point, I started actually doing some work and I ended up with
[this commit](https://github.com/xKDR/Survey.jl/commit/ce96f42bae02353d99116c23dacb0dc4f9844e35).
This was the beginning of the new design, started on the branch `design_update`. For now,
this branch still exists, and all of our work following this commit was done on this branch.
Hopefully, it will get merged into `main` soon.

## What actually happened in the `design_update` branch?
The design was updated. Simple enough. Except it _wasn't_ simple at all. When I started
working on this cool new design, I was naive enough to think that it will as easy as
changing all the code we have so far to accommodate for the new version. But _no_. With
new design comes new responsibility. Now we had to think about how to write elegant and
efficient code while still getting correct results with our package. Of course "elegant"
and "efficient" code writing comes with time and experience. Although getting correct results
is something that could be done right away. Unfortunately, we just weren't able to get
them right. And we are still [working on that](https://github.com/xKDR/Survey.jl/pull/51).
We did manage to change all our code and make it work for the new design. We just need
to correct it now.

## Documenting
.. is hard. Fortunately, the [Documenter.jl](https://juliadocs.github.io/Documenter.jl/stable/)
package makes it easier. Unfortunately, it takes time to read the documentation for
Documenter.jl and, as you can imagine, when you leave this part to the very end you are
rather short on time. But I did my best, I did my research, I went through the documentation
and through the source code of the documentation side by side and I came up with
[something](https://github.com/xKDR/Survey.jl/pull/54). Most functionality was already
documented. I just had to do some rephrasing here and there and restructure the final
document. Although it is starting to take a nice shape, I think.

## Testing
This is probably the weakest point of the package so far. Our code coverage is only 57%.
This definitely needs to be improved as soon as possible. After documenting everything,
increasing code coverage is my next goal.

## Final thoughts
I believe I made my thoughts about this experience clear until this point, but I
will explicitly say this anyway: it was great! Everything, every setback, every nice
comment, every documentation page has taught me a lot. I learned about survey statistics,
statistics software and people's expectations about statistics software (which are not
great - people expect it to be slow or expensive), about working with a very cool team
(thank you xKDR, and special thank you, Ayush!) and tons about Julia and starting a
package in Julia. For example, I found out there's a way to create
[package templates](https://invenia.github.io/PkgTemplates.jl/stable/). There is also a
very useful [formatting package](https://domluna.github.io/JuliaFormatter.jl/dev/), or
[testing functionality](https://github.com/JuliaLang/julia/blob/master/stdlib/Test/src/Test.jl).
I found a lot of cool plotting packages, like [Gadfly](http://gadflyjl.org/stable/),
[Makie](https://makie.juliaplots.org/stable/) or
[AlgebraOfGrpahics](https://juliaplots.org/AlgebraOfGraphics.jl/dev/). There was also
a lot to learn about the Julia style. Here I can share [this example](https://discourse.julialang.org/t/best-practice-default-values-optional-arguments-readable-code/24226)
of a discussion on positional arguments with default values versus keyword arguments.
I even found out there is the possibility of [ahead-of-time compilation](https://github.com/JuliaLang/PackageCompiler.jl/).

There is not enough space here to describe everything I learned, and even if there were,
most things I don't remember on the spot. I just realise I know them whenever I don't
search online for an answer. And there are other things which just can't be described.
Therefore, I will end this entry of my journal here, with a big THANK YOU to everyone
who guided me and was part of my journey in any way!
