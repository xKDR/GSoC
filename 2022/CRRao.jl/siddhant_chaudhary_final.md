# GSoC 2022 Final Report: Making improvements in JuliaStats/CRRao.jl

**Contributor:** Siddhant Chaudhary ([codetalker7](https://github.com/codetalker7))  
**Project URL**: https://github.com/xKDR/CRRao.jl  
**GSoC Project Page:** https://summerofcode.withgoogle.com/programs/2022/projects/wbPGeN3c

Overall, this project was about making improvements to the package in terms of the following.

- Good code organization.
- Introducing relevant structures and functions.
- Writing documentation. 
- Linting and refactoring the code.
- Testing the code. 

We will now describe the specific changes that were made keeping in mind the above goals.


## Code organization and utilities

The code is roughly organized as follows: the main types and other structures are defined in `src/CRRao.jl`. Two important types, viz. `FrequentistRegression` and `BayesianRegression` are defined in the file `src/fitmodel.jl`; the idea behind these types is two collect all frequentist regression models and all Bayesian regression models under the same umbrella. The `FrequentistRegression` type is a wrapper around the following data:

1. `model`: a model returned by the [GLM.jl](https://github.com/JuliaStats/GLM.jl) package.
2. `formula`: a `FormulaTerm`, which is used while making predictions from the model.
3. `link`: a property containing information about the link function which was used. 

`BayesianRegression` has a similar structure; instead of a model we have a `chain` object returned by the [Turing.jl](https://turing.ml/stable/) package.

This is the key idea behind our package: to introduce relevant structures to utilize different packages under the same API.

A few printing functions have been written in the file `print.jl`; again, the idea is to have a common format printed out, irrespective of what kind of statistical inference is done. A lot of improvements can be made to these printing functions.

The files `random_number_generator.jl` and `general_stats.jl` contain code for implementing a common RNG and some simple functions which are used in the code for the models.

Finally, the Bayesian models are in the `bayesian` directory, and the frequentist models are in the `frequentist` directory.

Look at commits [1ecbe78ec9b69ecf093f3241050844e57c88c322](https://github.com/xKDR/CRRao.jl/commit/1ecbe78ec9b69ecf093f3241050844e57c88c322) through [1726c30d9b2b99bce9c7ed3a1bca5c5c7acea626](https://github.com/xKDR/CRRao.jl/commit/1726c30d9b2b99bce9c7ed3a1bca5c5c7acea626) for the major chunk of the reorganization of the code.

## Relevant structures and functions

The main work is done by the `fitmodel` function of the package, which uses multiple dispatch to delegate the work to different handlers; the idea is to somehow replicate the `zelig` function from the [Zelig Project](http://docs.zeligproject.org/articles/quickstart.html) in R.

The `fitmodel` function supports a bunch of different signatures; currently these are 
```julia
@fitmodel(formula, data, modelClass)
@fitmodel(formula, data, modelClass, link)
@fitmodel(formula, data, modelClass, prior)
@fitmodel(formula, data, modelClass, link, prior)
```
As the package is expanded, more signatures will be added.

Along with this, we have a few getter functions in `src/frequentist/getter.jl` and `src/bayesian/getter.jl`; these functions are simply used to extract information from the trained models. The idea is to keep a structure like this for future additions to the package as well.

## Writing documentation

The package didn't have documentation; so we added a lot of it. Roughly it consists of a simple user guide to help a user get started with the package, and an API reference, which is supposed to be very detailed. The plan is to also include case studies in the documentation, and it is still being decided how that will be done. Look at commits [87b815ee8f479218e96f9892015069553009c5b4](https://github.com/xKDR/CRRao.jl/commit/87b815ee8f479218e96f9892015069553009c5b4) through [003821104aee649b5d9a8547fc797be78ef9085f](https://github.com/xKDR/CRRao.jl/commit/003821104aee649b5d9a8547fc797be78ef9085f) for the documentation.

## Linting and refactoring the code

The code has been refactored as much as possible to reduce repetitions and to ease the process of introducing changes. Currently, the best linting package for Julia seems to be [JuliaFormatter.jl](https://domluna.github.io/JuliaFormatter.jl/dev/), which is well-maintained and seems to have a good user base. We have used this package to prettify the code; however it seems that there are certain issues with [JuliaFormatter.jl](https://domluna.github.io/JuliaFormatter.jl/dev/) which need to be fixed before the process can be fully automated (i.e. be used with Git Hooks).

## Testing the code

Testing the package is not as straightforward as it originally seemed. For all the models, the goal is to test against the [Zelig](http://docs.zeligproject.org/articles/quickstart.html) package in R. However, this is a bit hard to achieve; for instance, for bayesian models which use [Turing.jl](https://turing.ml/stable/), running MCMC chains on different machines can lead to very different results because of differences in numericals. Currently, we only have a few basic checks which make sure that the code is running without throwing errors. Testing against [Zelig](http://docs.zeligproject.org/articles/quickstart.html) will require more thought and experiments. 
