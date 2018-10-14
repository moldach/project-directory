# A best-practices workflow for setting up a new data science project directory in the terminal or R

## GNU/Linux Terminal

### Setting up the project directory in the terminal

Create a project directory and nested subdirectories with a one-liner in the Terminal:

    mkdir -p hsapiens-snps/{data, refs, src, bin, analysis, figures}
    
* All of your raw data goes into `raw`  # Compress (`gzip file.fasta`) and read-only (`chmod 400 file.fasta`)
* All of your post processed data goes into `tidy` 
* Reference Data (*e.g.* Genomes/Transcriptomes) go into `refs`
* Source code for downloaded tools go into `src`
* Scripts you've written and compiled binaries go into `bin`
* All of the intermediary files generated, results/figures should go into analysis folder. 
    * You can then logically seperate subprojects (e.g. `sequencing-data-qc`, `alignment-results-qc`, `diff-exp-analysis`, etc.)

## The README file

Place a `README` file in each directory explaining how code is organized (*e.g.* `data/README.txt` contains metadata about all data files in data directory). For each filename, I recommend to have  short description of what the file is for. Document your methods/workflow, definition of code and symbols used to deal with missing data, include URL's for references to publication or external data and document when you download data (database release number/version number/names, etc.) and if you used MySQL or UCSC Genome browser to download data.

Update the `README` file as you work. As you create new files and folders, add their descriptions to the `README`. Include a list of variables, units of measurement of each variable, and information about how others should cite your analysis.

## File naming

* Avoid spaces: `2018 eda report.md` is not a good name but `2018_eda_report.md` is.
* Avoid punctuation, periods and other special characters except for underscores and dashes.
* Always use lower case: `Esophogael-Cancer_Report.md` is not a good name but `esophogael-cancer_report.md` is.
* Use leading zeros (left-pad) for correct ordering of files with `ls`: Use `file-021.txt` **NOT** `file21.txt`.
    * The numbers are determined by how many files you will potentially have. So if you may not have more than 1000 files you can choose three digits. If not more than a hundred files choose two digits and so on. 
    
Create dated directories/files using the command date `+%F` for ISO 8601 (`yyyy-mm-dd`). If you keep a digital notebook you can `Ctrl+F` to look for something see what date you did it and easily find that subdirectory.

    mkdir results-$(date +%F)
    
## RStudio

For some tasks with larger scale data projects, it will be easier to work with files programmatically in R rather than at the Terminal. For example, if you are working with hundreds of different files, moving them to a location that depends on information in the files is much easier with a programming language such as `R` rather than at the Terminal. Also, to document your work going from raw files to finished products, a good approach is to combine your data work in R with your file organization work in `R`.

### Useful functions

In `R` use the `file.create()` function. To verify that the file has been created use `list.files()`. Use `file.rename()` function to change names or the `file.remove()` to remove files, and `file.exists()` to check if a file exists. You can use the `dir.create()` function to create directories in `R` as well.

### Using relative paths make sharing projects easier

Relative paths for each project are the correct approach because it will allow a new person to follow along and run all your code without any problems. If you used absolute paths that work for your computer, rather than relative paths, none of the paths will make any sense on the person's computer whom you've shared your content with.

The `here` package is specifically designed to help you deal with file organization, allowing you to define in which folder all your relative paths should begin within a project. You set your project directory using `here()`, this function looks to see if you have a `.Rproj` file in your project then sets your base directory to whichever directory that file is located.

If you wanted to include a path to a file named "exploratory_code.R" in your `data/raw` code directory, you would simply specifiy that in your code like this:

    here("data", "raw", "exploratory_code.R")

Each subdirectory or file in the path is in quotes and simply separated by commas within the `here()` function and the output from this code includes the correct file path to this file. Use `here()` to set the base project directory for each data science project you do. And also use relative paths throughtout your code any time you want to refer to a different directory or sub-directory within your project using the syntax above.

### Write in a modular way

It is recommended that you write code in a modular way so that each group of code that do similar things can be put in a single file and the master file calls these individual files. The result is a much cleaner and shorter master file.

## Template for pieces of data analysis

Not every data analysis is the same and but this is a useful template for what the pieces of a data analysis are and how they flow together.

+ Define question
+ Determine what data you can access
+ Obtaining the data
+ Cleaning the data
+ Exploratory data analysis
+ Statistical prediction/modeling
+ Interpretation of results
+ Challenging of results
+ Synthsis and write up
+ Creating reproducible code

### Markdown

Use `knitr` for reproducible reports. `knitr` is ideal for

+ Manuals
+ Short/medium-length technical documents
+ Tutorials
+ Reports, especially if they will be generated periodically with updated data
+ Data preprocessing documents and summaries

`knitr` is **not** well-suited for 

+ Very long research articles
+ Complex and time-consuming computations (better to use literatite programming in conjuction with `Makefiles`)

Chunk caching is one way to avoid lengthy computations. By setting the `cache = TRUE` chunk option, it stores the output in a database in your working directory. Then, when you re-knit the document, instead of running the code in that particular chunk it re-loads the stored output from the database. By default dependencies between chunks are not checked. If the results of a cached chunk depend on a previous chunk that has been modified, those changes will not necessarily propogate down to later cached chunks.

Digitially document your work in R Markdown or Pandoc to render markdown to HTML

    pandoc --from markdown --to html notebook.md > output.html

## Sources

* [A Quick Guide to Organizing Computational Biology Projects](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424)
* [Best Practices for Scientific Computing](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1001745)
* [Good Enough Practices in Scientific Computing](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1005510)
* [The Biostar Handbook](https://www.biostarhandbook.com/)
* [Organizing Data Science Projects](https://leanpub.com/universities/courses/jhu/cbds-organizing)
* [Designing projects](https://nicercode.github.io/blog/2013-04-05-projects/), by [Rich Fitzjohn](https://nicercode.github.io/about/#Team)
* [How I Start A Bioinformatics Project](http://lab.loman.net/2014/05/14/how-i-start-a-bioinformatics-project/), by [Nick Lowman](http://lab.loman.net/about/)
* [Report Writing for Data Science in R](https://leanpub.com/reportwriting), by [Roger D. Peng](http://www.biostat.jhsph.edu/~rpeng/)
* [here, here](https://github.com/jennybc/here_here), by [Jenny Bryan](https://www.stat.ubc.ca/~jenny/)
* [here documentation](https://github.com/r-lib/here), by [Kirill Muller](https://github.com/krlmlr)
