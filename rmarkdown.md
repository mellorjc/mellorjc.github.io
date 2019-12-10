# Writing academic papers in rmarkdown that produce docx outputs

In this note I will be making use of [rmarkdown](https://rmarkdown.rstudio.com/), [bookdown](https://bookdown.org/), [Huxtable](https://hughjonesd.github.io/huxtable/), and [Lua pandoc filters](https://pandoc.org/lua-filters.html).
Another good resource is [rmarkdown for scientists](https://rmd4sci.njtierney.com/).



The pipeline for compiling an rmarkdown document is as follows.
![The various stages of compiling a rmarkdown file down to a PDF or a word document](rmarkdown_compile.svg)

The rmarkdown file is passed to knitr which runs the code blocks and stitches the results back into a markdown file. 
The markdown file is then passed to pandoc where the type of output file desired is specified. 
- If a pdf is specified then a TeX file is produced and this is passed to LaTeX compiler such as pdflatex which produces the final pdf document. 
- If a word document is specified then pandoc outputs the corresponding docx file.

Most resources I have found for producing scientific articles from rmarkdown generally assume that a pdf is the final output.
In this case often the path of least resistance is to solve rmarkdown formatting problems you have but introducing the appropriate LaTeX into the rmarkdown document. However, fixing you markdown formatting problems with LaTeX only helps when you are producing a pdf. What we really want is to fix out problems in the pipeline before an output is produced from pandoc. In this way we can output to pdf, and word documents (as well as html documents) with the same rmarkdown file.

## Producing tables

For producing tables we will use [Huxtable](https://hughjonesd.github.io/huxtable/). Some notes on using huxtable to produce tables are [here](https://mellorjc.github.io/huxtable/huxtable_example.html)

## Producing title pages with multiple authors

This can be achieved using [Lua pandoc filters](https://pandoc.org/lua-filters.html). Pandoc filters are a powerful mechanism to customise the output from pandoc by modifying the abstract syntax tree of the document.
There are some filters already written than will allow you to output documents with academic affiliations in the title page. These filters are [scholarly-metadata](https://github.com/pandoc/lua-filters/tree/master/scholarly-metadata) and [author-info-blocks](https://github.com/pandoc/lua-filters/tree/master/author-info-blocks). 
[More information on using these specific filters can be found here](https://www.r-bloggers.com/rmarkdown-template-that-manages-academic-affiliations-docx-or-pdf-output/)

To use these filters you specify the lua filters in your rmarkdown YAML header using the pandoc_args option for your output document type. 
For example to produce a word document the following can be added to your YAML header.

```
output: 
    bookdown::word_document2:
        pandoc_args:
            - --lua-filters=scholarly-metadata.lua
            - --lua-filters=author-info-blocks.lua
``` 



[Here is a blog with more information on making lua pandoc filters](https://ulyngs.github.io/blog/posts/2019-02-19-how-to-use-pandoc-filters-for-advanced-customisation-of-your-r-markdown-document/)


## Moving figures and tables to the end of the document


