# Census Resources
Census data with R

## Table of Contents

1. [Census Overview and Resources](#census-overview-and-resources)
2. [Getting started with tidycensus](#getting-started-with-tidycensus)
3. [Github and Command Line Basics](#github-and-command-line-basics)

## Census Overview and Resources

Here are some good resources for learning about the US Census and working with Census data.

1. [UVA Library Census Basics](https://guides.lib.virginia.edu/censusWorkshop): This resource includes basic information about the two most commonly used Census surveys, the Decennial Census and the American Community Survey (ACS), as well as Census geographies.

2. [Census Reporter](https://censusreporter.org/): This resource is focused on the American Community Survey (ACS) and is especially helpful for exploring [Census topics](https://censusreporter.org/topics/) and finding [Table Codes](https://censusreporter.org/topics/table-codes/) (like for [Race and Hispanic Origin](https://censusreporter.org/topics/race-hispanic/), and [Income and Earnings](https://censusreporter.org/topics/income/), and [Housing](https://censusreporter.org/topics/housing/)).

3. [data.census.gov](https://data.census.gov/): This is the US Census Bureau's search interface for exploring Census data, and is helpful in exploring tables and validating data pulled from other sources, like R tidycensus.

4. [Social Explorer Data Dictionary](https://www.socialexplorer.com/data/metadata): This is a helpful resource for finding and/or validating specific table codes. Scroll the Surveys on this page to find the specific data dictionary, for example [ACS 2022 (5-Year Estimates)](https://www.socialexplorer.com/data/ACS2022_5yr/metadata/?ds=ACS22_5yr&table=B01001H). From there, under Survey Details, click on the Dataset ([American Community Survey 2022](https://www.socialexplorer.com/data/ACS2022_5yr/metadata/?ds=ACS22_5yr)). This page provides the full list of tables - click on the desired one for the full details ([Table: B01001H. Sex By Age (White Alone, Not Hispanic Or Latino)](https://www.socialexplorer.com/data/ACS2022_5yr/metadata/?ds=ACS22_5yr&table=B01001H))  

5. [Glossary of Census geography terms](https://www.census.gov/programs-surveys/geography/about/glossary.html): This includes helpful definitions of Census geography areas, like [census tract](https://www.census.gov/programs-surveys/geography/about/glossary.html#par_textimage_13), [block group](https://www.census.gov/programs-surveys/geography/about/glossary.html#par_textimage_4) and [Public Use Microdata Areas (PUMA)](https://www.census.gov/programs-surveys/geography/about/glossary.html#par_textimage_22).

## Getting started with tidycensus

Follow this [guide on tidycensus basics](tidycensus-basics.md) for the first steps in using the R package tidycensus, and do some simple practice pulling Census data.

For a more complete guide to working with Census data in R, see [Analyzing US Census Data: Methods, Maps, and Models in R by Kyle Walker](https://walker-data.com/census-r/index.html)

## Github and Command Line Basics

Learn the basics for cloning a Github repo, pushing and pulling updates to/from the remote repository and command line for Mac and Windows users. See documentation: [Working with the Github Repo and Command Line](command-line.md)
