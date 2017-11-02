# A best-practices workflow for setting up a new project directory in linux

Create nested directories with this useful one-liner

    mkdir -p hsapiens-snps/{data/seqs,refs,bin,src,analysis}
    
* Reference Genomes/Transcriptomes go into "refs"
* Source code for downloaded tools go into "src"
* Scripts you've written/compiled binaries go into "bin"
* All of your raw data goes into "seqs"  # Make your data compressed (gzip file.fasta) and read-only (chmod 400 file.fasta)
* All of the intermediary files generated, results/figures should go into analysis folder. You can then logically seperate subprojects (e.g. sequencing-data-qc, alignment-results-qc, diff-exp-analysis, etc.)

Create dated directories using the command date +%F for ISO 8601. If you keep a digital notebook you can Ctrl+F to look for something see what date you did it and easily find that subdirectory.

    mkdir results-$(date +%F)

Place a README file in each directory explaining what it is (e.g. data/README.txt) contains metadata about all data files in data directory
Document your methods/workflow, include full command lines, include URL's for sources of external data and document when you download data (database release number/version number/names, etc.) and if you used MySQL or UCSC Genome browser to download data.

Use leading zeros for correct ordering of files with "ls". For example use file-0021.txt NOT file21.txt

Digitially document your work in R Markdown or Pandoc (in linux) to render markdown to HTML

    pandoc --from markdown --to html notebook.md > output.html
    
    
## Sources

* <http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424>
* <https://nicercode.github.io/blog/2013-04-05-projects/>
* <http://lab.loman.net/2014/05/14/how-i-start-a-bioinformatics-project/>
* <https://www.biostarhandbook.com/>
