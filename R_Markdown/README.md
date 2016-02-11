#Tutorial for generating parameterized report(s) in R, using [knitr](https://cran.r-project.org/web/packages/knitr/index.html) and [RMarkdown](http://rmarkdown.rstudio.com/) 

##Possible use cases:  
If you have to generate a report for multiple countries/sites/businesses, etc. that needs to be updated periodically as new data becomes available, this is a good option. For example: quarterly macroeconomic reports for multiple countries; quarterly earnings reports for multiple product lines; quarterly program evaluation reports for multiple hospitals. In each use case, it  is assumed that the format is the same across comparable units, or that the format can be predictably altered via conditional/branching logic. 

**Step 1:** If you haven't already, install the required packages.

    install.packages(c("knitr", "rmarkdown"))

**Step 2:** Create a new .R file

Define your parameters: what are the units or groups that you need to iterate through to produce your reports? Real world examples of this might include: countries, hospitals, business units, reporting quarters, etc. To make this as easy as possible, we will use an R dataset, JohnsonJohnson. You could easily replace this dataset with one you import yourself and convert to a data.frame (from Excel, STATA, SAS, etc.), or generate within R.

    # The dataset we will work with displays Johnson & Johnson quarterly earnings per share, from 1960 to 1980
    test <- JohnsonJohnson
    
    #For the sake of demonstration, let's say we want to produce a very simple report for each quarter, where we 
    #plot EPS from 1960 to 1980

    # Put the paramter you want to loop over into a list
    quarters <- colnames(JohnsonJohnson)
    
    # Set up a call to render R markdown while looping through quarters
    # The .Rmd is a markdown file; we will create it next; for now, insert the path/file name you intend to use,
    # w/extension ".Rmd" The output file should correspond to the name you want to call each file; 
    # if you don't differentiate, each run will overwrite previous contents.
    # Output here is a .docx; html and pdf are also options. 
    # output_dir is the directory where you want to save the quarterly reports you will generate
    
    for(i in 1:length(quarters)){
    rmarkdown::render("YourDirectory/JandJ_Tutorial.Rmd", params = list(quarter = quarters[i], data=test),
                       output_file =  paste(quarters[i], "_JandJtest.docx", sep=''), 
                       output_dir = 'insertYourDirectoryHere')
    }

**STEP 3:** Create the corresponding .Rmd file

A few things to note: (1) Your title needs to match the title of the argument you pass in your .R file. Based 
on what we did in Step 2, the title of our .Rmd file should be "JandJ_Tutorial". (2) The params field of the <a href = "https://en.wikipedia.org/wiki/YAML" YAML </a> portion of your .Rmd file need to match the params you define in your render 
call, and be instantiated in some way (here, they are instantiated as empty strings).





