# Create a R package

For in depth information on how to a package works, see [R Packages](https://r-pkgs.org/) by [Hadley Wickham](https://hadley.nz/) and [Jenny Bryan](https://jennybryan.org/).

</br>

All instructions are checked using the following conditions.
- **OS**: Windows 11
- **R version**: 4.2.2 (2022-10-31 ucrt) -- "Innocent and Trusting"
- **RTools**: Version 4.2 installed
- **UI**: Rstudio 2022.07.2+576 Spotted Wakerobin (desktop)
- **Package versions**:
    - **devtools**: 2.4.5 (CRAN)
    - **roxygen2**: 7.2.2.9000 (GitHub)
    - **testthat**: 3.1.5 (CRAN)
    - **knitr**: 1.41 (CRAN)

</br>

## Setup

1. Install and load `devtools` and other necessary packages.
```r
    install.packages(c("devtools", "roxygen2", "testthat", "knitr"))
    library(devtools)
```

2. Create the package. Replace ``<< path >>`` with the path where the package should be created. *If the path exists, it is used. If it does not exist, it is created, provided that the parent path exists. See [documentation](https://usethis.r-lib.org/reference/create_package.html) for more details.*
```r
    create_package(path = "<< path >>")
```

Alternatively, we can add fields to the [`DESCRIPTION` file](https://r-pkgs.org/metadata.html#sec-description) at the same time as creating the package. Replace all `<< ... >>` with proper details.
```r
    create_package(
        path = "<< path >>",
        fields = list(
            `Title` = "<< title >>",
            `Authors@R` = 'person("<< first name >>", "<< last name >>", 
                          email = "<< email >>", 
                          role = c("aut", "cre"), 
                          comment = c(ORCID = "<< orcid >>"))',
        `Description` = "<< description >>"
        )
    )
```

3. Set the package licence. See more details about package licences [here](https://r-pkgs.org/license.html). I used MIT.
```r
    use_mit_license(copyright_holder = "<< name >>")
```

4. Add additional packages used in the package. **This step can be done even later as we create the package.** In this example, I use [`dplyr`](https://dplyr.tidyverse.org/).
```r
    use_package("dplyr")
```

5. Create a README file. You can also use `use_readme_md()` if you just need a simple file.
```r
    use_readme_rmd()
    build_readme()
```


## Creating R functions

All functions should be written inside an R file (with extension `.R`). Each R file can include one or more functions.

1. Create a R file.
```r
    use_r(name = "<< file name >>")
```

2. Add the function to the newly added file.

3. Add the function documentation with *roxygen2*. See [this](https://r-pkgs.org/man.html) on how to. If you are using RStudio, you can click on the function and select `Code -> Insert Roxygen Skeleton` to add a sample template.

4. Repeat these steps for all files.

5. Check if the package loads properly. *This roughly simulates what happens when a package is installed and loaded with `library`. See [documentation](https://www.r-project.org/nosvn/pandoc/devtools.html) for more details.*
```r
    load_all()
```

6. updates the documentation, builds and check the package. *See [documentation](https://www.r-project.org/nosvn/pandoc/devtools.html) for more details.* 
```r
    check()
```

> This step can fail if you are saving the package to a network drive (at least on Windows with roxygen2 7.2.2). To mitigate this issue, install the development version from [GitHub](https://github.com/r-lib/roxygen2).
> ```r
> devtools::install_github("r-lib/roxygen2")
> ```


## Testing

See more information on testing and using the `testthat` package, [here](https://r-pkgs.org/testing-basics.html).

1. Setup testing.
```r
    use_testthat()
```

2. Setup a series of tests for each R file.
```r
    use_test("<< file name >>")
```

3. Run tests with `test()`. Alternatively, it will be run automatically when you call `check()`.


## Optional

### Badges

Adds badges to the README. See the [documentation](https://usethis.r-lib.org/reference/badges.html) for more details. Make sure to build the README file again with `build_readme()`, after adding badges.

```r
use_lifecycle_badge("experimental")
```

### Package website

Use GitHub to easily create a website for the package. Git should be installed on the computer for this to work.

1. Get a personal access token (PAT). Run this and follow instructions on browser. See [documentation](https://usethis.r-lib.org/articles/git-credentials.html) for more information.
```r
    create_github_token()
```

2. Insert your PAT in the Git credential store.
```r
    gitcreds::gitcreds_set()
```

3. Setup GitHub to publish the site on GitHub.
```r
    use_pkgdown_github_pages()
```

4. Build the site.
```r
    build_site()
```

### Code of conduct

```r
use_code_of_conduct(contact = "<< email >>")
```

### Create a changelog

```r
use_news_md()
```

### Increase version

```r
use_version(which = "minor")
```
