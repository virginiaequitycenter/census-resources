# R tidycensus Basics

This guide[^1] provides step-by-step instructions for getting started with the `tidycensus` package and some practice using basic `tidycensus` functions. To follow along, open a new R script in RStudio and copy/paste/type the code as instructed below. 

For a complete tutorial in using `tidycensus` and using US Census data, see [Analyzing US Census Data: Methods, Maps, and Models in R](https://walker-data.com/census-r/index.html), by Kyle E. Walker. 

## Table of Contents

1. [Introduction to tidycensus](#introduction-to-tidycensus)
2. [Your Census API key](#your-census-api-key)
3. [Get Census data](#get-census-data)
   - [get_decennial()](#get_decennial)
   - [get_acs()](#get_acs)
4. [Searching for variables](#searching-for-variables)

## Introduction to `tidycensus`

[`tidycensus`](https://walker-data.com/tidycensus/) is an R package designed to help R users get Census data pre-prepared to use with tidyverse. `tidycensus` is an R package that allows users to interface with a select number of US Census Bureau’s data APIs and return tidyverse-ready data frames, optionally with simple feature geometry included.

To install the package from an R environment, type and run the following command once:

```
# Only run ONCE (first time using package on local system)
install.packages("tidycensus")
```

After the first time installing the package, any future scripts that reference the package can have this line of code:

```
# Run at the start each time working with R script
library(tidycensus)
```

## Your Census API key

To access ACS data programmatically, you will need to obtain an API key from the Census Bureau. This key allows you to make requests to the Census API and retrieve data directly into your R environment. To gain access to your own API key, access the website [here](https://api.census.gov/data/key_signup.html). This site will require an organization name (or personal name of individual) and an email address. Activate the key from the email you receive from the Census Bureau so it works correctly.

Set your Census API key with the `census_api_key()` function. Declaring `install = TRUE` when calling census_api_key() will install the key for use in future R sessions.

```
census_api_key("YOUR KEY GOES HERE", install = TRUE)
```

**NOTE:** Your API key is private information, take care to not share the string value or make it publicly available (our Github repositories are public). One way to ensure your key does not get pushed to the remote repository is by adding your Census API key to your R environment variables. The steps below will walk you through this process:

1. Locate your `.Renviron` file. This is a hidden file that is likely located in your home directory. You can confirm the location of your home directory by running `Sys.getenv("HOME")` in your Rstudio console:

```
> Sys.getenv("HOME")
[1] "/Users/eam5hc"
```

2. Open `.Renviron` in a text editor (For Mac, TextEdit or Windows, Notepad)

3. Add the following line and save the file:

```
CENSUS_API_KEY='YOUR KEY GOES HERE'
```

4. Now you can access your API key from your environment variables, and never need to include them directly in your R script. Use the `Sys.getenv()` command to bring the `CENSUS_API_KEY` variable into you script:

```
# Create an object to store your Census API key for the session
census_api <- Sys.getenv("CENSUS_API_KEY")

# Run the census_api_key() function using that object
census_api_key(census_api, install = TRUE)
```

## Get Census data

After your API key is installed, you can obtain decennial Census or ACS data with a single function. Let’s start with get_decennial(), which is used to access decennial Census data from the 2000, 2010, and 2020 decennial US Censuses.

### get_decennial()

In your R console, run `?get_decennial()` to see the definitions of the arguments used in this function. You only need to include the arguments where your value is different from the default. Below is a list of some of the arguments we typically use in our work.

**geography**
This is the requested Census geography level for the data. We most frequently use `"state"`, `"county"`, `"tract"`, and `"block group"`.

**variables**
This is where the variable ID, or list of variable IDs go. This will return both the estimate and margin of error for the given variables.

**year**
The year you are requesting data. The default is set to the most current year data is available. This is 2020 for the decennial Census.

**state**
The state for the data you are requesting. We are typically interested in Virginia, and use the FIPS code `"51"` for our work.

**county**
The county or counties for the data. Include a single county FIPS code (example, for Charlottesville use `"540"` ), or a list of counties (example, for Charlottesville and Albemarle use `c("540", "003")`).

**output**
This defines the how the data frame is built. The default `"tidy"` provides a "long" table of the variables, while `"wide"` makes a column for each variable. We often use `"wide"` in our work, but both are useful depending on what you plan to do with the data.


Here is an example. We'll request the total population for each state ([Table P1](https://data.census.gov/table/DECENNIALPL2020.P1)) for the most recent decennial Census (2020):

```
# Request decennial census data
total_population_2020 <- get_decennial(
  geography = "state", 
  variables = "P1_001N",
  year = 2020
)

#View datatable
View(total_population_2020)
```

View the output table, and validate against the same table on [data.census.gov](https://data.census.gov/). This example pulled the total population from the table P1:Race in the 2020: DEC Redistricting Data (PL 94-171), seen [here](https://data.census.gov/table/DECENNIALPL2020.P1).

### get_acs()

More frequently, we use data from the American Community Survey (ACS) in our work. In your R console, run `?get_acs()` to see the definitions of the arguments used in this function (they are similar to the ones above).

Run the below example, where we request ACS data on the percent of the population that is under 18 years by census tract for the City of Charlottesville from the most recent ACS 5-year survey (2018-2022).

```
# Request ACS data
under_18 <- get_acs(
    geography = "tract",
    variables = "S0101_C02_022", 
    state = "VA", 
    county = "540", 
    survey = "acs5",
    year = 2022
)

# View datatable
View(under_18)
```

View the output table, and validate against the Census website table values, [here](https://data.census.gov/table?q=S0101&g=050XX00US51540,51540$1400000_1400000US51540000201,51540000202,51540000302,51540000401,51540000402,51540000501,51540000502,51540000600,51540000700,51540000800,51540000900,51540001000). This table has been filtered to include columns for all census tracts in Charlottesville. The table code "S0101_C02_022" refers to the row "Under 18" under the subsection "SELECTED AGE CATEGORIES". Scroll across the table to see the "Percent Estimate" and "Margin of Error" values for each tract.

Compare the data table above to another with the `output` argument set to `"wide"`:

```
# Request ACS data
under_18_wide <- get_acs(
    geography = "tract",
    variables = "S0101_C02_022", 
    state = "VA", 
    county = "540", 
    survey = "acs5",
    year = 2022,
    output = "wide"
)

# View datatable
View(under_18_wide)
```

## Searching for variables

To find the variable ID(s) for the `variables` argument in get_decennial() and get_acs(), use the function `load_variables()`. Run `?load_variables()` in your console to read more usage of this function. Use the arguments for `year` and `dataset`, and include `cache = TRUE` to store for future access.

Let's practice by obtaining datasets of variables from the 5-year ACS (2018-2022):

```
# Variables from 5-year ACS Detailed Tables
acs_var <- load_variables(2022, "acs5", cache = TRUE)

# View acs_var
View(acs_var)

# Variables from 5-year ACS Subject Tables
acs_var_subject <- load_variables(2022, "acs5/subject", cache = TRUE)

# View acs_var_subject
View(acs_var_subject)

# Variables from 5-year ACS Data Profile
acs_var_profile <- load_variables(2022, "acs5/profile", cache = TRUE)

# View acs_var_profile
View(acs_var_profile)
```

One of the easier ways to identify a specific variable ID from the above data tables, is by exploring [Table Codes](https://censusreporter.org/topics/table-codes/) using Census Reporter and searching the data table in RStudio.

For example, if we want to find the variable ID for the number of foreign-born residents in a geography, first we can use Census Reporter to identify the table with that variable. In this case, it is [Table B05002: Place of Birth by Nativity and Citizenship Status](https://censusreporter.org/tables/B05002/).

Now we can search our data table of variables (`acs_var`) to filter for table `B05002`. Open `acs_var` in the view window in RStudio, and in the search bar in the upper right, type in `B05002`.

Scroll until you see the row and label you're looking for. In this case it is: `Estimate!!Total:!!Foreign born:`.

The `name` column gives you the specific variable ID to use in your `get_acs()` function. For this example, it is `B05002_013`.

```
# A custom R function that creates a table of all variable codes and metadata
all_acs_meta <- function(year){
  
  # Gets the list of all variables from all acs5 metadata tables
  vars1 <- load_variables(year, "acs5") %>% select(-geography) # Remove the geography column
  vars2 <- load_variables(year, "acs5/profile")
  vars3 <- load_variables(year, "acs5/subject")
  vars4 <- load_variables(year, "acs5/cprofile")

  # Provides column with specific lookup
  vars1$dataset_table <- "acs5"
  vars2$dataset_table  <- "acs5/profile"
  vars3$dataset_table  <- "acs5/subject"
  vars4$dataset_table  <- "acs5/cprofile"

  # Combine all table rows
  all_vars_meta <- rbind(vars1, vars2, vars3, vars4)

  return(all_vars_meta)
}

# Creates a table of all the metadata called "meta_table"
meta_table <- all_acs_meta(year = 2021)

# Opens the newly made table
View(meta_table)
```

[^1]: This guide was authored by Elizabeth Mitchell, with content adapted from the documentation [Pulling and Organizing Census ACS Data](https://virginiaequitycenter.github.io/cdf-united-way-2023-24/Documentation/combined-county-documentation.html) by Ethan Assefa & Sanny Yang.
