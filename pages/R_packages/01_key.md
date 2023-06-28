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
- **`document()`**: 
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

- 1st paragraph - Title no longer than a sentenc
- 2md paragraph - Treated as a brief description of the function
- 3rd paragpraph - Details section where more inf

- R directory
