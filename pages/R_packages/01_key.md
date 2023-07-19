## Key componenets of a R package

Although the are several files and directories that an R package can have
only 4 elements are critical:

- **`R` directory**: where you keep all of your R code
     - No subdirectroies allowed
     - For small projects for 1 file per function if you can
     - For larger projects group similar functions in same file 
- **`man` directory**: Where all the packages documentation is stored
- **`NAMESPACE` file**: Info regarding functions that your packge imports from 
  other packages and functions that the package makes available to users
- **`Description` file**: Contains metadata about what package does, it's version and dependencies etc.

***

Similarly to design an R package there are several important tools
that we can use to automate some of the processing. The are:

- **`devtools`**: helps with package structure
- **`roxygen2`**: helps with documentation and management of the NAMESPACE file

***

## Key devtools functions

- **`create()`**: Creates the basic package structure (doesn't create the **`man`** directory)
- **`document()`**: Creates / updates `.rd` files for all functions / data etc. (extracted from
  details input into the `roxygen` headers) and saves them in the man directory. Also updates the `NAMESPACE` file.
- **`check()`**: Runs a series of quality control checks before you build your package
- **`build()`**:
- **`test()`**:

***

## Abbr. for authors etc

- **`ctb`**: contributor
- **`cph`**: copyright holder
- **`aut`**: author
- **`cre`**: package maintainer

***

## Optional directories

- **`Data`**: data to be included with your packages
- **`Vingettes`**: tutorials for how to 
- **`Tests`**: Unit tests for the functions in your packes
- **`Compiled code`**: i.e. C++ code
- **`Translations`**: for messages
- **`Data`**:

***

## Data

- **`use_data()`** - Add data to R package
- **`use_vingette()`** - Add `Rmd` vingette template to R package
- **`use_package()`** - Add package to depnds, suggests or import fields of Discription file


## roxygen2

Clear and informative documentation is essential for the end user to understand 
how to use your package effectively. `roxygen2` allows you to add a header to 
a function to explain what the function does. When you then run the `build` 
function help files are generated based on the content of function headers. 

***

### Function documentation 

The minimum documentation for a function is :

- Title sentence
- Description paragraph
- Arguments
- Exported (for exported functions only)

- ***

- **1st paragraph**:Title no longer than a sentenc
- **2nd paragraph**: Treated as a brief description of the function
- **3rd paragpraph**: Provide detail on params and functionali
- **`@param`**: Tag for function argument. Useful to mention object type here.
- **`@import`**: Tag for extern packages that need to be imported for function
  to work properly.
- **`@importFrom`**: If you want to import a single function from a package.
- **`@export`**: Functions that are visible to the end user. Utility functions
  used by your package are not usually not exported directly. Exported
  functions are automatically added to the `NAMESPACE` file after using `build()`.
- **`@returns`**:
- **`@examples`**: Example code to demostrate how the function should be used by
  the user. These examples are checked for errors when `check` is run.
  - If don't want an example function to be run during `check()` add
  the `\dontrun { function() }` tag around the example function.
- **`return`**: Tag for what the function output is.
  - You can use `\code` tag to refer to one fo the parameters of the function.
- **`author`**: Who wrote the function
- **`seealso`**: to link to closely related functions
- **`notes`**: Additional notes, such as upcoming changes etc.
- **`\code{text to format}`**: Add code to your docs.
- **`\link[packageName]{functionName}`**: Add link to another function
  `packageName` is only needed if the functionn does not is from another
  package.
- **`\itemize{}`** returns an unordered list of `\items` following this
  syntax `\itemize{ \item ...}` to describe elements of an object.

***

### Package documentation

This header can go in an file but it is good practive to save it in
a file called `<package name>.R` in the R directory.

- **"_PACKAGE"**: This keyword header needs to be follow package headers
- **"@doctype"**: write package here
- **"@name"**: package name

***

### Data documentation

To create a data object in your package use:

`devtools::use_data(my_data, pkg = "my_package")` 

This saves a compressed copy of my_data in the `data` directory.

This header can go in an file but it is good practive to save it in
a file called `<package name>.R` in the R directory.

- **@format**: i.e. A data.frame with 10 cols.This keyword header needs to be follow package headers
- **`\describe`**: Describe the format of the data similar to `itemize`:
    ```R
    #' Random Weather Data
    #'
    #' A dataset containing randomly generated weather data.
    #'
    #' @format A data frame of 7 rows and 3 columns
    #' \describe{
    #'  \item{Day}{Numeric values giving day of the week, 1 = Monday, 7 = Sunday}
    #'  \item{Temp}{Numeric values giving temperature in degrees Celsius}
    #'  \item{Weather}{Character values describing the weather on that day}
    #' }
    #' @source Randomly generated data
    "weather"
    ```
- **"source"**
- The name of the data object is placed at the bottom of the header to in quotes

### Checking an R package

Basic quality control process before you build a package. Crucially running `check()` does not 
check that the code you have written in your scripts runs as expected, that is what unit 
testing is for, it runs over 50 checks such as chekcing that:

- The package is installable
- Dependecies are available
- There are no syntax errors in the code
- That the documentation is complete
- That the the Discription info is correct
- Tests estscan be run and passed
- Vingettes can be built

In terms of the output that `check()` generates:

- An error is something that must be fixed as it will cause a problem for the end user
- A warning is less problematic, as the package will likely work but should be fixed
- Notes are less problematic again but if publishing to CRAN you will have to justify
  why these have not been fixed

***

### Difference between depends, imports, suggests

The DESCRIPTION file has several fields relating to packges dependencies. 

- Depends: Loaded with the package   
- Imports: Not loaded with package but they are required for the function package to
work properly
- Suggests: These packages are not loaded or required but may be useful extensions for
  the package

Imports is necesaary to get around function masking that can occur when several packages 
with the same function name are loaded. It is best practice to use `imports` to list 
package dependencies and then to call functions like so: `dplyr::select()` in your code.
Normally it is just the requred version of R that is listed in depends. 

***

## Continuous integration

When building your package you can create a source package or a binary. 

- A source package ends in `tar.gz`.
- A binary file is a special file of pre-compiled code that is ready to be used on the
  operating system it was built on (Windows / Mac). It doesn't need any special build
  commands to get it running.

Continuous integration

- Automatically checks whenever you make a change to you code
- Used with version control
- Runs every time you make an update
- When you include tests CI reruns all tests to make sure any changes to code do not
  cause test to fail
- travis CI is a common CI implemetation and can be implemented on git use using
  `devtools::use_travis(<package_name>)`

***

## Unit testing

Why write unit tests?

A function that works correctly now may not behave as expected in the future if:

- Supporting or connected code could be added or modified
- A later version of R and / or later versions of packegs are used
- The code is run on new data
- The code is run on a different OS

Running `use_testthat` creates a `test` directory in the packge root and generates a
script called `testthat.R` which contains the code to run the tests. There is also a
`testthat` directory created in the `test` dir where test scripts should be saved. 

The `expect_*` functions are used during unit testing. Some examples are:

- **`expect_identical()`** checks that the values, attributes, and type of both objects
  are the same.
- **`expect_equal()`** checks that the values, and attributes of both objects are the
  same. You can adjust how strict `expect_equal()` is by adjusting the tolerance parameter.
- **`expect_equivalent()`** checks that the values, of both objects are the same.
- **`expect_error()`** tests if running a function returns an error.
- **`expect_warning()`** tests if running a function returns a warning.
- **`expect_output()`** tests that output generated by function is as expected.
- **`expect_output_file()`** tests whether the output of a function matches the content of
  a file. This is useful for testing longer outputs. The first time you run this test you
  will get an error message as the file has not been generated. It is usually run using
  `update = TRUE` first.

Grouping tests:

To run tests in packages you need to group these individual tests together. You do this using 
a function `test_that()`. You can use `context()` to collect these groups together. You usually 
have one context per file. This  makes it easier to work out where failing tests are located.

```R
# Use context() and test_that() to group the tests below together
context("Test data_summary()")

test_that("data_summary() handles errors correctly", {

  # Create a vector
  my_vector <- 1:10

  # Use expect_error()
  expect_error(data_summary(my_vector))

  # Use expect_warning()
  expect_warning(data_summary(airquality, na.rm = FALSE))

})
```

