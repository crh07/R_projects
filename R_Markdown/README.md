#Tutorial for generating parameterized report(s) in R, using [knitr](https://cran.r-project.org/web/packages/knitr/index.html) and [RMarkdown](http://rmarkdown.rstudio.com/) 

##Possible use cases:  
If you have to generate a report for multiple countries/sites/businesses, etc. that needs to be updated periodically as new data becomes available, this is a good option. For example: quarterly macroeconomic reports for multiple countries; quarterly earnings reports for multiple product lines; quarterly program evaluation reports for multiple hospitals. In each use case, it  is assumed that the format is the same across comparable units, or that the format can be predictably altered via conditional/branching logic. 

**Step 1:** If you haven't already, install the required packages.

    install.packages(c("knitr", "rmarkdown"))

**Step 2:** Create a new .R file

Define your parameters: what are the units or groups that you need to iterate through to produce your reports? Real world examples of this might include: countries, hospitals, business units, reporting quarters, etc. To make this as easy as possible, we will use an R dataset, mtcars. You could easily replace this dataset with one you import yourself and convert to a data.frame (from Excel, STATA, SAS, etc.), or generate within R.

The dataset we will work is *mtcars*; it was extracted from the 1974 Motor Trend US magazine, and comprises fuel consumption 
and 10 aspects of automobile design and performance for 32 automobiles (1973–74 models)
    
    test <- mtcars # here, test is a data frame. 
    
For the sake of demonstration, let's say we want to produce a very simple report for each engine type, filtering on number of cylinders. First, examine the distribution of this variable:

     unique(test$cyl) 

Put the paramter you want to loop over into a vector:

     cylinders <- sort(unique(test$cyl))
    
Set up a call to render R markdown while looping through these cylinder "types"
The .Rmd is a markdown file; we will create it next; for now, insert the path/file name you intend to use,
w/extension ".Rmd" The output file should correspond to the name you want to call each file; if you don't differentiate, 
each run will overwrite previous contents. Output here is a .docx; html and pdf are also options. 
output_dir is the directory where you want to save the quarterly reports you will generate
    
    for(i in 1:length(cylinders)){
    rmarkdown::render("YourDirectory/mtcars_Tutorial.Rmd", params = list(cylinders = cylinders[i], data=test),
                       output_file =  paste(cylinders[i], "_mtcars_Tutorial.docx", sep=''), 
                       output_dir = 'insertYourDirectoryHere')
    }

**STEP 3:** Create the corresponding .Rmd file

A few things to note: (1) Your title needs to match the title of the argument you pass in your .R file. Based 
on what we did in Step 2, the title of our .Rmd file should be "mtcars_Tutorial". (2) The params field of the <a href = "https://en.wikipedia.org/wiki/YAML" > YAML </a> portion of your .Rmd file need to match the params you define in your render 
call, and be instantiated in some way (here, our params are "cylinders" and "data", and they are instantiated as empty strings). The YAML portion is separated from the rest of the code with "---", as shown below:

    ---
    title: "mtcars_Tutorial"
    author: "crh07"
    date: "February 18, 2016"
    output: word_document
    params:
     cylinders: 
          ""
     data:
          ""
    ---

The benefit of using Markdown and parameterizing your code is that you can generate customized documents that have text/plots in common, but that may need specific data points, plots, and unit name fields (i.e. car/country/hospital "x") to change each time. **You're essentially using a "for" loop to generate all the reports you need in a way that is scalable and very easy to update**, provided the input data format and output content format don't change from iteration to iteration. For example, in our .Rmd file, we might have a block of text that is the same for all the reports, and doesn't require customization or alteration:

This is an example of a generic paragraph that we want to use in ALL the reports we generat, without customizing: 

    "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."










