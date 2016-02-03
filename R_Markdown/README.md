#Tutorial for generating a (series of) parameterized report(s) in R, using knitr and RMarkdown 

##Possible use cases:  
If you have to generate a report for multiple countries/sites/businesses, etc. that needs to be updated periodically as new data becomes available, this is a good option. For example: quarterly macroeconomic reports for multiple countries; quarterly earnings reports for multiple product lines; quarterly program evaluation reports for multiple hospitals. In each use case, it  is assumed that the format is the same across comparable units, or that the format can be predictably altered via conditional/branching logic. 

*Step 1:* If you haven't already, install the required packages.

    install.packages(c("knitr", "rmarkdown"))

