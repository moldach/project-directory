<a href="https://images.unsplash.com"><img src="https://images.unsplash.com/photo-1527849214787-c99cd25c2f09?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=400&q=60" title="ProjectOrganization" alt="ProjectOrganization"></a>

Best Practices for Data Science Project Workflows and File Organization
=======================================================================

:star: Star us on GitHub — it helps!

To ensure replicability and traceability of scientific claims, it is
essential that the scientific publication, the corresponding data sets,
and the data analysis are made publicly available. All software should
be openly shared and directly accessible to others. Publications should
highlight that the code is freely available in, for example, Github.
This openness can increase the chances of getting the paper accepted for
publication. Journal editors and reviewers receive the opportunity to
reproduce findings during the manuscript review process, increasing
confidence in the reported results. Once the paper is published, your
work can be reproduced by other member of the scientific community,
which can increase citations and foster opportunities for further
discussion and collaboration.

Basic workflow of data analysis
-------------------------------

------------------------------------------------------------------------

Not every data analysis is the same and but this is a useful template
for what the pieces of a data analysis are and how they flow together.

-   Define question
-   Determine what data you can access
-   Obtaining the data
-   Cleaning the data
-   Exploratory data analysis
-   Statistical prediction/modeling
-   Interpretation of results
-   Challenging of results
-   Synthesis and write up
-   Creating reproducible code

Creating a project skeleton
---------------------------

------------------------------------------------------------------------

### Linux

------------------------------------------------------------------------

Create a project directory and nested sub directories with a one-liner
in the `Terminal`:

``` bash
mkdir -p hsapiens-snps/{data/{raw,tidy},refs,src,bin,analysis,figures}  # no spaces between subdirectories
```

![terminal GIF](http://g.recordit.co/iA5Q38T49I.gif)

-   All of your raw data goes into `raw` \# Compress (`gzip file.fasta`)
    and read-only (`chmod 400 file.fasta`).
-   All of your post processed data goes into `tidy`
-   Reference Data (*e.g.* Genomes/Transcriptomes) go into `refs`
-   Source code for downloaded tools go into `src`
-   Scripts you’ve written and compiled binaries go into `bin`
-   All of the intermediary files generated, should go into `analysis`
    folder.
    -   You can then logically separate sub projects
        (e.g. `sequencing-data-qc`, `alignment-results-qc`,
        `diff-exp-analysis`, etc.)

### R

------------------------------------------------------------------------

There are a few differences between standard skeletons on terminal and
`R`.

-   A `bin` folder is not required for `R` projects
-   Scripts should be kept in the `R` directory. Ideally you should have
    one script to run everything if possible in the main directory or a
    `Makefile`
-   A `man` directory that contains the manual (*i.e.* documentation)
    for the use of the functions in your package (*optional directory if
    using `roxygen2`, you should!*)
-   Github is not designed for hosting large data sets, either host them
    externally or provide a small-sample data set that people can try
    out the techniques without having to run very expensive computations

Create `New Project` with `RStudio` (*e.g.* hsapiens-snps) and then
create the sub folders

``` r
library(purrr)
library(here)

folder_names <- here(c("data", "refs", "R", "analysis", "figures", "man")) # base-level folders
folder_names <- c(folder_names, here("data", c("raw", "tidy"))) # add data subdirectories
# sapply(folder_names, dir.create)  # base R way
walk(folder_names, dir.create) # purrr-fect way
```
![R GIF](http://g.recordit.co/JGiwsAbnLs.gif)

In `R` use the `file.create()` function. To verify that the file has
been created use `list.files()`. Use `file.rename()` function to change
names or the `file.remove()` to remove files, and `file.exists()` to
check if a file exists. You can use the `dir.create()` function to
create directories in `R` as well.

If you project is only based in `R` you should create one folder per
project with the files organized like this:

``` r
name_of_project
|--data
    |--raw
        |--file_001.xlsx
        |--file_002.gen
        |--file_002.sample
    |--tidy
        |--file_001-cleaned.csv
|--refs
    |--Oldach_2018.pdf
|--analysis
    |--01-analysis.Rmd
|--figures
    |--01-scatterplot.jpg
    |--01-correlation.png
|--R
    |--exploratory_analysis.R
    |--pdf_scraper.R
|--name_of_project.Rproj
|--run_all.R
```

### File naming

------------------------------------------------------------------------

-   Avoid spaces: `2018 eda report.md` is not a good name but
    `2018_eda_report.md` is.
-   Avoid punctuation, periods and other special characters except for
    underscores and dashes.
-   Always use lower case: `Esophogael-Cancer_Report.md` is not a good
    name but `esophogael-cancer_report.md` is.
-   Use leading zeros (left-pad) for correct ordering of files with
    `ls`: Use `file-021.txt` **NOT** `file21.txt`.
    -   The numbers are determined by how many files you will
        potentially have. So if you may not have more than 1000 files
        you can choose three digits. If not more than a hundred files
        choose two digits and so on.

Create dated directories/files using the command date `+%F` for ISO 8601
(`yyyy-mm-dd`). If you keep a digital notebook you can `Ctrl+F` to look
for something see what date you did it and easily find that sub
directory.

``` bash
mkdir results-$(date +%F)
```

Git with Github
---------------

For every project you *should* create a repository on Github (Gitlab,
Bitbucket, SourceForge, *etc.*) to take advantage of branches & pull
requests, code snapshots, tracking project bugs and enhancements using
issues, and dissemination of the final results. The recommendations
below should be broadly applicable to most repository hosting services.
These recommendations take full advantage of Github’s features for
managing and promoting projects in bioinformatics as well as in many
other research domains.

> Github repositiores are freely visible to all. Many projects decide to
> share their work publicly and openly from the start of the project in
> order to attract visibility and to benefit from contributions from the
> community early on. Private “repos” on Github/Gitlab are used to
> ensure that work is hidden but also limit collaborations to just those
> users who are given access to the repository. These repos can then be
> made public at a later stage, for example upon submission, acceptance,
> or publicaiton of corresponding journal atricles.

[A useful gist for setting up version control and Github can be found
here](https://github.com/marskar/respect) and the above was taken from
the great introduction to Git/Github by [Perez-Riverol *et al.,*
2016](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004947).

Every repository should have ideally the following three files (in
addition to the code):

-   The `LICENSE` file that clearly defines the permissions and
    restrictions attached to the code and other files in your
    repository.
-   The `README` file which provides a short description of the project,
    a quick start guide, information on how to contribute, a TODO list,
    and links to additional documentation.
-   The `CITATION` file to the repository informs your users how to cite
    and credit your project.

The `LICENSE` file
------------------

------------------------------------------------------------------------

Common licenses such as those listed on <http://choosealicense.com> are
preferred. The `LICENSE` file in the repository should be a plain-text
file containing the contents of an [Open Source Initiative
(OSI)-approved license](https://opensource.org/licenses/alphabetical),
not just a reference to the license.

The `README` file
-----------------

------------------------------------------------------------------------

A `README.md` file that describes the overall project and where to get
started. It may be helpful to include a graphical summary of the
interlocking pieces of the project.

In each directory explaining how code is organized (*e.g.*
`data/README.txt` contains metadata about all data files in data
directory). For each file name, I recommend to have a short description
of what the file is for. Document your `methods/workflow`, definition of
code and symbols used to deal with missing data, include `URL`’s for
references to publication or external data and document when you
download data (database release number/version number/names, *etc*.) and
if you used `MySQL` or `UCSC Genome browser` to download data.

Update the `README` file as you work. As you create new files and
folders, add their descriptions to the `README`. Include a list of
variables, units of measurement of each variable, and information about
how others should cite your analysis.

[Browse some examples of awesome `README`s
here](https://github.com/matiassingers/awesome-readme)!

The `DESCRIPTION` file
----------------------

A `DESCRIPTION` file in the project root provides formally structured,
machine- and human-readable information about the authors, the project
license, the software dependencies, and other metadata of the
compendium. When yore compendium is an R package, you can take advantage
of many time-saving tools for package development, testing, and sharing
(*e.g.* the `devtools` package).

The `NAMESPACE` file
--------------------

A `NAMESPACE` file in the project root provides that provides the
exports the functions in a package, so that they can be used in the
current project and by other packages.

Creating a research commpendium
-------------------------------

------------------------------------------------------------------------

Temple, Lang and Gentleman (2007) advanced the proposal for using the R
package structure as a “Research Compendium,” an idea that has since
caught on with many others [(Marwick *et al.,*
2018)](https://doi.org/10.1080/00031305.2017.1375986).

-   [Papers with publicly available data sets may receive a higher
    number of citations than similar studies without available
    data](https://doi.org/10.1080/00031305.2017.1375986)
-   Data sharing is associated with higher publication productivity
-   A compendium makes it easier to communicate our work with others,
    including collaborators and (not least of all) our future selves

It may be tempting to organize all scripts and reports with ascending
names, for example `001-load.R`, `002-clean.R` *etc.* and although it
helps with organization it does not capture the full tree of
dependencies. In the terminal it would be best to use a `Makefile` to
manage more complex workflows as they capture the full tree of
dependencies and control the order of code execution.

On `R` I would strongly recommend using the [Drake
package](https://github.com/ropensci/drake) which I’ve talked about
recently: [“Getting Started with Drake in
R”](https://moldach.github.io/xaringan-presentation_drake/#1).

If your workflow combines `R` code, `terminal` and `python` I would
suggest using [`snakemake`](https://snakemake.readthedocs.io/en/stable/)
or
[`Airflow`](https://towardsdatascience.com/getting-started-with-apache-airflow-df1aa77d7b1b).
(*TO DO*: look into the `reticulate` package and see how complex
multi-language workflows are managed)

### Examples of real-world research compendia using R packages

-   [Hollister *et al.,*
    2016](https://github.com/USEPA/LakeTrophicModelling)
-   [Boettiger *et al.,*
    2015](https://github.com/cboettig/nonparametric-bayes)

The Boettiger and colleagues example provides a more complex example of
a large compendia. This example includes a `.travis.yml`, a
configuration file for the Travis continuous integration service, a
`Dockerfile` that instructs the Docker software how to create a virtual
environment for running the R code in an isolated and portable context,
and the `Makefile` which has instructions for executing R code in the
compendium in a way that avoids unnecessary repetition. These three
files contain organizing metadata that are intended to be both machine-
and human-readable, but are not written in the R language.

Unit tests, contained in the `tests` directory, are R code scripts to
test that specific functions in the compendium produce their expected
outputs, given known inputs. The addition of these tools, initially
developed for software engineering, solves the problems of specifying
the computational environment and the relationships between data, code,
and output.

The top-level `manuscripts` directory of Boettiger *et al.,* holds the
files that generate the journal article and the supplementary documents,
as well as the `Makefile` and `Dockerfile`. Other notable items at the
top-level of this compendium are the `.drone.yml`, `.zenodo.json` and
`.traivs.yml` files. The drone file contains configuration details for
the Drone continuous integration service that operates in Docker
containers. In this case, each time a commit is made to the repository,
the Drone web service automatically renders the manuscript files
(including running the R code in the manuscript) to PDF in a Docker
container defined by the `Dockerfile` in `manuscripts/`. This provides
an automatic check to see if a PDF can be generated from the manuscript
R markdown files after the last commit. The `.travis.yml` file performs
a similar purpose, but is focused on whether or not the R package can be
successfully build in a generic Linux environment. The `.zenodo.json`
file provides machine-readable metadata about the compendium. This is
useful for automatically archiving the compendium at zenodo.

### Automatically create project skeletons in R and integrate

------------------------------------------------------------------------

The [`starters` package](https://github.com/lockedata/starters) on
Github provides a number of functions, leveraging the `usethis` package,
designed to take away *some* of the grunt work involved in setting up
new projects. The `createAnalysisProject()` function creates a project
ready for a typical analysis project with `packrat` to manage
dependencies to keep things consistent and reproducible, while the
`createPackageProject()` helps setup a project with code coverage,
vignettes, unit testing *etc.* out of the box.

Dedicated code testing is aimed at detecting possible bugs introduced by
new features or changes in the code or dependencies, as well as
detecting wrong results, known as *logic errors*, in which the source
code produces a different result than what was intended. Continuous
integration provides a way to automatically and systematically run a
series of tests to check integrity and performance of code, a task that
can be automated through Github. `starters` sets up \[Travis
CI\](<a href="https://Travis-ci.org/" class="uri">https://Travis-ci.org/</a>,
a hosted continued integration platform that is free for all open-source
projects. Travis CI builds and test the source code using a plethora of
options such as different platforms and interpreter versions.

By organizing files into an R package, you follow conventions that save
you time thinking about the best way to organize your project. Writing
functions for packages makes it easy to document the use of code
(particularly if you use `roxygen2`). This documentation can help you be
productive more quickly when returning to work on a project after
stepping away from it.

As a project grows in size and collaboration, having the more rigid
structure offered by a package format becomes increasingly valuable.
Packages can automate installation of dependencies, perform checks that
changes have not broken code, and provide a modular and highly tested
framework for collecting together the three essential elements: data,
code, and manuscript-quality documentation, in a powerful and
feature-rich environment.

> When you iteratively develop a package, you can easily run
> comprehensive checks on your code with `R CMD check` at the terminal
> or `devtools::check()` at the R console. Regular checking your package
> like this can help identify problems before they become very
> frustrating to solve.

Packages are meant to be distributed, so viewing a project as a package
means preparing your project to be distributed. This can help keep you
honest when programming; you’re less likely to take shortcuts when you
plan on releasing it to the world. Furthermore, others can start using
the tools you made and that can earn you citations or even an additional
paper in [*J. Stat. Soft.*](https://www.jstatsoft.org/index) or the [*R
Journal*](https://journal.r-project.org/).

### Template for scholarly writing (`RMarkdown` and `bookdown`)

------------------------------------------------------------------------

Another tool I have not yet tried yet, but looks promising, is
[`rrtools`](https://github.com/benmarwick/rrtools). It provides a
convenient starting point for writing a journal article, report, or
thesis.

### Publish your analysis to a website

------------------------------------------------------------------------

The traditional way to promote scientific software (and manuscripts) is
by publishing a paper in the peer-reviewed scientific literature.
Additional steps can boost the visibility of an organization. For
example Github *Pages* are simple websites freely hosted by Github. User
can create and host blog websites, help pages, manuals, tutorials, and
websites related to specific projects. Pages come with a powerful static
site generator called [Jekyll](https://jekyllrb.com/) that can be
integrate with other frameworks as [Boostrap](http://getbootstrap.com/)
or platforms such as [Disqus](https://disqus.com/) to support and
moderate comments.

There are also real-time communication systems such as
[Gitter](http://gitter.im/) that allows the user community, developers,
and project collaborators to exchange ideas and issues to report bugs or
get support. Within a Gitter chat, members can reference issues,
comments, and pull requests. Github also supports wikis for each
repository.

The [`workflowr`
package](https://cran.r-project.org/web/packages/workflowr/index.html)
combines literate programming (`knitr` and `rmarkdown`) and version
control (`Git` via `git2r`) to generate a website containing
time-stamped, versioned, and documented results.

Blog Post
---------

A much shorter version of this article [appeared originally on
Medium](https://towardsdatascience.com/the-gold-standard-of-data-science-project-management-13d68c9e85d6)

Acknowledgments
---------------

------------------------------------------------------------------------

-   [A Quick Guide to Organizing Computational Biology
    Projects](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424)
-   [Best Practices for Scientific
    Computing](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1001745)
-   [Good Enough Practices in Scientific
    Computing](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1005510)
-   [The Biostar Handbook](https://www.biostarhandbook.com/)
-   [Organizing Data Science
    Projects](https://leanpub.com/universities/courses/jhu/cbds-organizing)
-   [Designing
    projects](https://nicercode.github.io/blog/2013-04-05-projects/), by
    [Rich Fitzjohn](https://nicercode.github.io/about/#Team)
-   [How I Start A Bioinformatics
    Project](http://lab.loman.net/2014/05/14/how-i-start-a-bioinformatics-project/),
    by [Nick Lowman](http://lab.loman.net/about/)
-   [Report Writing for Data Science in
    R](https://leanpub.com/reportwriting), by [Roger D.
    Peng](http://www.biostat.jhsph.edu/~rpeng/)
-   [here, here](https://github.com/jennybc/here_here), by [Jenny
    Bryan](https://www.stat.ubc.ca/~jenny/)
-   [here documentation](https://github.com/r-lib/here), by [Kirill
    Muller](https://github.com/krlmlr)
-   [File organization best
    practices](https://andrewbtran.github.io/NICAR/2018/workflow/docs/01-workflow_intro.html?utm_content=buffer858fd&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer)
    by [Andrew Tran](https://twitter.com/abtran)
-   [ropensci/rrrpkg: tools and templates for making research
    compendia](https://github.com/ropensci/rrrpkg#useful-tools-and-templates-for-making-research-compendia)
-   [How to share data with a
    statistician](https://github.com/jtleek/datasharing) by [Jeff
    Leek](http://jtleek.com/)
-   [Ten Simple Rules for Taking Advantage of
    Github](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004947)
