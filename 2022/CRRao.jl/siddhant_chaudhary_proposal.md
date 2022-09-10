# GSoC Proposal for CRRao.jl

## Overview of the Proposal

My name is [Siddhant Chaudhary](https://codetalker7.github.io) (visit the link to see my homepage), and I’m currently a student at the Chennai Mathematical Institute (in India). My Github username is [codetalker7](https://github.com/codetalker7). In this document, I’ll be laying out a plan of action to work on the [CRRao.jl](https://github.com/xKDR/CRRao.jl) package to improve it by adding new features to it, making it more efficient, and adding necessary documentation and other requirements to it. You can find more details about my technical background and contributions in later sections.

## Detailed Description of Ideas

### Adding new models to CRRao.jl

Currently, CRRao.jl only includes four regression models (namely Linear, Logistic, Poisson and Negative Binomial Regression). New models can be added to CRRao:

1. Gamma and Inverse Gaussian Regression models.
2. ARIMA model (time series).

There are plenty of other models that can be added, but the work can be initiated by adding these.

### Metaprogramming

There is a scope of adding macros to the package to make the syntax even cleaner. An example of this can be given. Consider the following code.

```julia
m = fitmodel(
    @formula(mpg ~ hp + wt + Gears),
    data,
    LinearRegression(),
    Optimization()
)
```

A macro can be written to change the above to the following.

```julia
m = fitmodel(
    mpg ~ hp + wt + Gears,
    data,
    LinearRegression(),
    Optimization()
)
```

### Performance Improvement via Lazy Computing

From the documentation, it can be seen that every model will have the following attributes.

- `model.sigma`  
- `model.LogLike`
- `model.AIC`  
- `model.BIC` 
- `model.R_sqr`
- `model.AdjustedRsqr`  
- `model.fittedResponse` 
- `model.residuals`
- `model.Cooks_distance`

When working with big data, it can become quite inefficient and expensive to compute all of the above attributes when the fit function of a model is called; in some cases, a user might not even need information about all of these attributes, and only a subset of them. In such cases, we can use lazy evaluation, i.e., we will compute these quantities as and when they are needed.

### Unit Testing, Documentation and Code Coverage

In its current state, CRRao.jl doesn't have any testing; rigorous unit testing can be added to the package to verify the validity of code. These tests can be designed from scratch, or the code can be tested against similar statistical implementations in R.

The documentation for the package can also use a lot of help. All of these steps can be automated using CI/CD, as done in other xKDR Forum packages (like NighttimeLights.jl, which was recently released in the Julia registry; check out my contributions section).

### Linting, fixing code conventions and model API documentation

This idea is optional, but is very important to maintain good quality code and to also help future contributors in the package. Currently, CRRao.jl doesn’t seem to follow any fixed code conventions or styles (examples are no variable name conventions, spacing, commit message conventions or documentation conventions); potential future contributors also don’t have any guide to follow. All of these conventions can be documented, and can be automatically enforced using a linting tool. Git hooks can be used to enforce these rules on future contributors before any commits are made. Also, the main idea of CRRao.jl, namely to have a consistent API for models is not well documented and can only be understood by going through the code. This should also be well documented so that new models can be added at ease.

A possible linting tool for this would be [Lint.jl](https://github.com/tonyhffong/Lint.jl).

### Expectations by the end of GSoC

My goal will be to complete all of the above mentioned ideas for CRRao.jl, and potentially to work on more features as the need arises.

## Previous Open Source Contributions

### Contributions in Julia

I have worked with the creators of CRRao.jl (xKDR Forum) in developing a few statistics packages for Julia. In particular, my contributions have been in the following projects.

1. [Lowess.jl](https://github.com/xKDR/Lowess.jl): In this package, we have implemented the standard LOWESS smoother using pure Julia code. This package is an alternative to the existing Loess.jl package, which implements the more general LOESS smoother. My job was to write the main code, write documentation, and run tests against a C implementation of the same function.

2. [NighttimeLights.jl](https://github.com/xKDR/NighttimeLights.jl): In this already existing package, there is a lot of scope of multithreading; for example, we could multithread the `aggregate_dataframe` function in `src/aggregate.jl`. My job was to verify if this is indeed the case by running tests on the multithreaded version of the code (check out the `multithreading` branch).

I also helped out in setting up the CI/CD for unit testing, documentation building and code coverage for these packages. All of this was done by making a new package which sets up a reusable package template (see https://github.com/xKDR/PkgTemplateXKDR.jl).

Recently I also helped in releasing the NighttimeLights.jl package in the Julia registry. I can do all of these steps for CRRao.jl as well, when it reaches a point where its initial version can be released.

###  Other Open Source Contributions

I have briefly contributed to the LibreOffice codebase, which is mostly in C++. Here are links to three of my submitted patches (out of which the first two have been merged, and the third is work in progress).

1. https://gerrit.libreoffice.org/c/core/+/133100 (A beginner level hack) 
2. https://gerrit.libreoffice.org/c/core/+/131408/1 (Another beginner level hack)
3. https://gerrit.libreoffice.org/c/core/+/132342 (An intermediate level hack; involved implementation of a UI based dialog).

## A little more about myself and my background

As I mentioned at the beginning of this document, I have a personal web page: https://codetalker7.github.io. I’m a mathematics and computer science undergrad student, and I love to code. I have a good mathematical background, which helps me a lot to solve problems (and this is something which I really enjoy). Here are some more of my details.

- Email: urssidd@gmail.commit
- GitHub profile: https://github.com/codetalker7 
- Resume (as of January 2022): https://drive.google.com/file/d/15SxGPUFkv216iwPirgOQpYgvFGEyH9Q2/view?usp=sharing 
