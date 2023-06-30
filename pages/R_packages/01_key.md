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
- **`check()`**:
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


