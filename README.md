# dataReporter 

<!---<img src="man/figures/logo.png" width="121px" height="140px" align="right" style="padding-left:10px;background-color:white;" />--->

[![Travis-CI Build
Status](https://travis-ci.org/ekstroem/dataReporter.svg?branch=master)](https://travis-ci.org/ekstroem/dataReporter)
[![CRAN\_Release\_Badge](http://www.r-pkg.org/badges/version-ago/dataReporter)](https://CRAN.R-project.org/package=dataReporter)
![Download counter](http://cranlogs.r-pkg.org/badges/grand-total/dataReporter)


dataReporter is an R package for documenting and creating reports on data cleanliness. 


## Installation

This github page contains the *development version* of dataReporter. For the
latest stable version download the package from CRAN directly using

```{r}
install.packages("dataReporter")
```

To install the development version of dataReporter run the following
commands from within R (requires that the `devtools` package is already installed)

```{r}
devtools::install_github("ekstroem/dataReporter")
```

## Package overview

A super simple way to get started is to load the package and use the
`makeDataReport()` function on a data frame (if you try to generate several
reports for the same data, then it may be necessary to add the `replace=TRUE`
argument to overwrite the existing report). 

```{r}
library("dataReporter")
data(trees)
makeDataReport(trees)
```

This will create a report with summaries and error checks for each
variable in the `trees` data frame. The format of the report depends on your OS and whether 
you have have a [LaTeX](https://www.latex-project.org/) installation on your computer, which
is needed for creating pdf reports. 


### Using dataReporter interactively

The dataReporter package can also be used interactively by running checks
for the individual variables or for all variables in the dataset

```{r}
data(toyData)
check(toyData$events)  # Individual check of events
check(toyData) # Check all variables at once
```

By default the standard battery of tests is run depending on the
variable type. If we just want a specific test for, say, a numeric
variable then we can specify that. All available checks can be viewed
by calling `allCheckFunctions()`. See [the
documentation](https://github.com/ekstroem/dataReporter/blob/master/latex/article_vol2.pdf)
for an overview of the checks available or how to create and include
your own tests.


```{r}
check(toyData$events, checks = setChecks(numeric = "identifyMissing"))
```

We can also access the graphics or summary tables that are produced for a variable by calling the `visualize` or `summarize` functions. One can visualize a single variable or a full dataset:

```{r}
#Visualize a variable
visualize(toyData$events)

#Visualize a dataset
visualize(toyData)
```  

The same is true for summaries. Note also that the choice of checks/visualizations/summaries are customizable:

```{r}
#Summarize a variable with default settings:
summarize(toyData$events) 

#Summarize a variable with user-specified settings:
summarize(toyData$events, summaries = setSummaries(all =  c("centralValue", "minMax"))  
```


## Detailed documentation

You can read the main paper accompanying the package at the [Journal
of Statistical
Software](https://www.jstatsoft.org/article/view/v090i06). It provides
a detailed introduction to the dataReporter package (original launched under the name `dataMaid`).

We also have two blog posts that provide an introduction to the package. The can be found [here (the primary one)](https://sandsynligvis.dk/2017/08/21/datamaid-your-personal-assistant-for-cleaning-up-the-data-cleaning-process/) and [here](https://sandsynligvis.dk/2018/03/03/generating-codebooks-in-r/).

Moreover, we have
created a vignette that describes how to extend dataReporter to include
user-defined data screening checks, summaries and visualizations. This
vignette is called `extending_dataReporter`:

```{r}
vignette("extending_dataReporter")
```




<!---## Online app

We are currently working on an online version of the tool, where users
can upload their data and get a report. A prototype
is already up and running - we just need to configure the R server correctly.

Until we have set it up online, you can try it out on your own machine:
```{r}
library(shiny)
runUrl("https://github.com/ekstroem/dataReporter/raw/master/app/app.zip")
``` 
--->