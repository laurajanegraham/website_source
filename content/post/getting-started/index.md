+++
title = "Building a website with blogdown"

date = 2018-05-27T00:00:00
lastmod = 2018-05-27T00:00:00
draft = false

tags = ["blogdown"]
summary = "Some resources for creating an academic website using blogdown and the Hugo academic theme."

[header]
image = "headers/hex-blogdown.jpg"
caption = ""
+++

Having seen a few nice looking websites which use the Hugo academic theme, I thought I'd have a go at revamping my website. I'm not going to write a whole new post about how to do this - there are plenty of these online. I just thought I'd point out some resources that I found useful. 

I mainly worked from Alistair Bailey's really detailed guide to [building a website using blogdown in R](http://ab604.github.io/docs/website_bookdown/). This was pretty comprehensive and I had a site I was happy with up and running very quickly. Two key things I needed from elsewhere were (1) how to upload the site to my existing GitHub pages address; and (2) how to get the `cite` buttons on publications that I'd seen on other sites. 

### Linking to GitHub pages

The main issue I had with building the site was linking it to my existing GitHub pages site. The guide I followed provided instructions on linking to Netlify. For anyone also wanting to link to GitHub pages, Tyler Clavelle explains how to do so with a [two repository approach](https://tclavelle.github.io/blog/blogdown_github/). 

### Cite button

To get the `cite` button next to the publication (along with `PDF` `Code` etc. buttons), there needs to be a separate `.bib` file for each publication in the `static/files/citations` directory. This file needs to have the same name as the `.md` file for the publication. I have modified the code by [Lorenzo Busetto](https://lbusett.netlify.com/post/automatically-importing-publications-from-bibtex-to-a-hugo-academic-blog/) (which creates the required `.md` files for each publication) to also output `.bib` files to `static/files/citations`: 

```
bibtex_2academic <- function(bibfile,
                             abstract = TRUE,
                             overwrite = FALSE) {
  require(RefManageR)
  require(dplyr)
  require(stringr)
  require(anytime)
  
  # Import the bibtex file and convert to data.frame
  mypubs   <-
    ReadBib(bibfile, check = "warn", 
            .Encoding = "UTF-8") %>%
    as.data.frame()
  
  # assign "categories" to the different types of
  # publications
  mypubs   <- mypubs %>%
    dplyr::mutate(
      pubtype = dplyr::case_when(
        bibtype == "Article" ~ "2",
        bibtype == "Article in Press" ~ "2",
        bibtype == "InProceedings" ~ "1",
        bibtype == "Proceedings" ~ "1",
        bibtype == "Conference" ~ "1",
        bibtype == "Conference Paper" ~ "1",
        bibtype == "MastersThesis" ~ "3",
        bibtype == "PhdThesis" ~ "3",
        bibtype == "Manual" ~ "4",
        bibtype == "TechReport" ~ "4",
        bibtype == "Book" ~ "5",
        bibtype == "InCollection" ~ "6",
        bibtype == "InBook" ~ "6",
        bibtype == "Misc" ~ "0",
        TRUE ~ "0"
      )
    )
  
  # function to create the filename
  create_filename <- function(x, ext) {
    filename <- paste(
      x[["year"]],
      x[["title"]] %>%
        str_replace_all(fixed(" "), "_") %>%
        str_remove_all(fixed(":")) %>%
        str_sub(1, 20) %>%
        paste0(ext),
      sep = "_"
    )
  } 
  
  # create a function which populates the md template 
  # based on the info
  # about a publication
  create_md <- function(x) {
    # define a date and create filename by appending date 
    # and start of title
    if (!is.na(x[["year"]])) {
      x[["date"]] <- paste0(x[["year"]], "-01-01")
    } else {
      x[["date"]] <- "2999-01-01"
    }
    
    filename <- create_filename(x, ".md")
    # start writing
    if (!file.exists(file.path("content/publication", filename)) |
        overwrite) {
      fileConn <- file.path("content/publication", filename)
      write("+++", fileConn)
      
      # Title and date
      write(paste0("title = \"", x[["title"]], "\""),
            fileConn,
            append = T)
      write(paste0("date = \"", anydate(x[["date"]]), "\""),
            fileConn,
            append = T)
      
      # Authors. Comma separated list, e.g. `["Bob Smith", 
      # "David Jones"]`.
      auth_hugo <-
        str_replace_all(x["author"], " and ", "\", \"")
      auth_hugo <-
        stringi::stri_trans_general(auth_hugo, "latin-ascii")
      write(paste0("authors = [\"", auth_hugo, "\"]"),
            fileConn,
            append = T)
      
      # Publication type. Legend:
      # 0 = Uncategorized, 1 = Conference paper, 
      # 2 = Journal article
      # 3 = Manuscript, 4 = Report, 5 = Book,  6 = Book
      # section
      write(paste0("publication_types = [\"", x[["pubtype"]],
                   "\"]"),
            fileConn,
            append = T)
      
      # Publication details: journal, volume, issue, 
      # page numbers and doi link
      publication <- x[["journal"]]
      if (!is.na(x[["volume"]]))
        publication <- paste0(publication,
                              ", (", x[["volume"]], ")")
      if (!is.na(x[["pages"]]))
        publication <- paste0(publication,
                              ", _pp. ", x[["pages"]], "_")
      if (!is.na(x[["doi"]]))
        publication <- paste0(publication,
                              ", ",
                              paste0("https://doi.org/",
                                     x[["doi"]]))
      
      write(paste0("publication = \"", publication, "\""),
            fileConn,
            append = T)
      write(paste0("publication_short = \"",
                   publication,
                   "\""),
            fileConn,
            append = T)
      
      # Abstract and optional shortened version.
      if (abstract) {
        write(paste0("abstract = \"", x[["abstract"]], "\""),
              fileConn,
              append = T)
      } else {
        write("abstract = \"\"",
              fileConn,
              append = T)
      }
      write(paste0("abstract_short = \"", "\""),
            fileConn,
            append = T)
      
      # other possible fields are kept empty. They can be
      # customized later by
      # editing the created md
      
      write("image_preview = \"\"",
            fileConn,
            append = T)
      write("selected = false", fileConn, append = T)
      write("projects = []", fileConn, append = T)
      write("tags = []", fileConn, append = T)
      #links
      write("url_pdf = \"\"", fileConn, append = T)
      write("url_preprint = \"\"",
            fileConn,
            append = T)
      write("url_code = \"\"", fileConn, append = T)
      write("url_dataset = \"\"",
            fileConn,
            append = T)
      write("url_project = \"\"",
            fileConn,
            append = T)
      write("url_slides = \"\"", fileConn, append = T)
      write("url_video = \"\"", fileConn, append = T)
      write("url_poster = \"\"", fileConn, append = T)
      write("url_source = \"\"", fileConn, append = T)
      #other stuff
      write("math = true", fileConn, append = T)
      write("highlight = true", fileConn, append = T)
      # Featured image
      write("[header]", fileConn, append = T)
      write("image = \"\"", fileConn, append = T)
      write("caption = \"\"", fileConn, append = T)
      
      write("+++", fileConn, append = T)
    }
  }
  # apply the "create_md" function over the 
  # publications list to generate
  # the different "md" files.
  
  apply(
    mypubs,
    FUN = function(x)
      create_md(x),
    MARGIN = 1
  )
  
  # Added section to output .bib files to get the cite button
  lapply(
    1:nrow(mypubs),
    FUN = function(x) {
      # define a date and create filename by appending date 
      # and start of title
      filename = create_filename(mypubs[x,], ".bib")
      bib <- as.BibEntry(mypubs[x,])
      WriteBib(bib, paste0("static/files/citations/", filename))
    }
  )
}
```

### Final thoughts and extra resources

To make tweaks to my site, I mostly just found it useful to have a bit of a look at other sites that have been built with blogdown. Often the source code is discoverable (for example for a `<username>.github.io` site, the code will be at `github.com/<username>/<username>.github.io`). There are lots of resources out there about using blogdown to build sites. Here are some examples: 

- [The blogdown package handbook](https://bookdown.org/yihui/blogdown/)
- [Up and running with blogdown, by Alison Presmanes Hill](https://alison.rbind.io/post/up-and-running-with-blogdown/): This guide also provides links to resources on using Git, RStudio and Terminal. 
- [Mikey Harper explains the why of migrating to blogdown](https://mikeyharper.uk/migrating-to-blogdown/)
- ...and many more out there. 

Happy website building! 