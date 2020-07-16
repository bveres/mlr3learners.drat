
# mlr3learners.drat

[`drat`](https://github.com/eddelbuettel/drat) package repository for
the
[mlr3learners.drat](https://github.com/mlr3learners/mlr3learners.drat)
project.  
Install mlr3 learners from this repository using

``` r
install.packages(..., repos = "https://mlr3learners.github.io/mlr3learners.drat")
```

To omit having to pass the `repos` argument every single time, add the
following to your `.Rprofile`:

``` r
options(repos = structure(c(getOption("repos"),
  mlr3learners = "https://mlr3learners.github.io/mlr3learners.drat")))
```

This will append this drat repo to your list of available repositories
which `install.packages()` will check when trying to install a package.
If you want a c/p command for this: The following will append the line
above to your `~/.Rprofile` while keeping your currently set CRAN
mirror:

``` r
cran = getOption('repos')[['CRAN']] 
write(sprintf("options(repos = structure(c(CRAN = '%s',
  mlr3learners = 'https://mlr3learners.github.io/mlr3learners.drat')))",
   cran),
  file = "~/.Rprofile", append = TRUE)
```

**Note: Restart your R session to make use of the new option.**

With this, you are able to install any learner from the
[mlr3learners](https://github.com/mlr3learners) organization as simple
as

``` r
install.packages("mlr3learners.kernlab")
```

## Available packages

List of all available learners in this organization

    ##  [1] "mlr3learners.c50"             "mlr3learners.coxboost"       
    ##  [3] "mlr3learners.extratrees"      "mlr3learners.flexsurv"       
    ##  [5] "mlr3learners.fnn"             "mlr3learners.gbm"            
    ##  [7] "mlr3learners.gss"             "mlr3learners.kerdiest"       
    ##  [9] "mlr3learners.kernlab"         "mlr3learners.ks"             
    ## [11] "mlr3learners.liblinear"       "mlr3learners.locfit"         
    ## [13] "mlr3learners.logspline"       "mlr3learners.mboost"         
    ## [15] "mlr3learners.nnet"            "mlr3learners.np"             
    ## [17] "mlr3learners.obliquersf"      "mlr3learners.partykit"       
    ## [19] "mlr3learners.penalized"       "mlr3learners.pendensity"     
    ## [21] "mlr3learners.plugdensity"     "mlr3learners.proba"          
    ## [23] "mlr3learners.randomforest"    "mlr3learners.randomforestsrc"
    ## [25] "mlr3learners.rweka"           "mlr3learners.sm"             
    ## [27] "mlr3learners.survival"        "mlr3learners.survivalsvm"

## How it works

The package sources and binaries of the listed packages are build by the
respective learner packages with the following logic:

  - During a CI run when the version does not contain a devel indicator
    (i.e. .9000)
  - On the first of each month

Binaries are build for the current R release and the previous release.
Depending on the release date of a new major or minor R version it may
take a while until the new binaries are available. The binaries are
(re-)build on the first of each month.

The work behind the scenes is highly simplified by packages
[{tic}](https://github.com/ropensci/tic) and
[{drat}](https://github.com/eddelbuettel/drat).

### How to add a mlr3learner to {mlr3learners.drat}

1.  Check if you belong to the [mlr3learners.drat
    team](https://github.com/orgs/mlr3learners/teams/mlr3learners-drat)
    of the mlr3learners organization. If not, ask @pat-s to add you -
    otherwise, you do not have permission to add deploy keys to this
    repository.

2.  Make sure you have a `GITHUB_PAT` set for the GitHub API calls. If
    you do not know what this is, read
    `?usethis::browse_github_token()`.

3.  Make sure your repo already uses GitHub Actions. If not, call
    `tic::use_ghactions_yml(deploy = TRUE)`.

4.  Source `init_mlr3learner_drat_deploy()` from [this
    Gist](https://gist.github.com/pat-s/be7b0ebc6953726d5a51a171742c3e21)
    and call the function with the name of the repo,
    e.g. `init_mlr3learner_drat_deploy("mlr3learners.mboost")`. This
    will add a private key as a “secret” to your repo and a public
    deploy key to
    [mlr3learners/mlr3learners.drat](https://github.com/mlr3learners/mlr3learners.drat),
    making it possible to deploy to the latter from your repo.

5.  Now the macOS and Windows runners will push their binaries to
    mlr3learners.drat for every no-devel version (i.e. versions without
    .9000 at the end). The Linux runner will push the `.tar.gz` file to
    install the learner from source.

**Note**

It is very important to only init one CI run per release and then switch
directly to a dev version. Otherwise, binaries will be pushed during
every CI run, bloating up the repo size of
mlr3learners/mlr3learners.drat\](<https://github.com/mlr3learners/mlr3learners.drat>).
