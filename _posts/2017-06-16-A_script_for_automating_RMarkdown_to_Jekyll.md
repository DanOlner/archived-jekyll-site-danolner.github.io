---
layout: post
title: "A script for automating RMarkdown to Jekyll"
date: 2017-06-16
comments: true
---
  
  




I'm going to be making a little more of an effort to get some coding up on this blog - not least because I'm always going, 'now where's that chunk of really useful code that does x and y?' and having it here would make it easier for me to find.

So to start with, here's something a little bit meta. I've been using RMarkdown on Windows to create blogposts for Jekyll, and part of that's been [using this little script](http://gtog.github.io/workflow/2013/06/12/rmarkdown-to-rbloggers/) to Knit the Post into jekyll-ready markdown.

So this post assumes you're already doing something similar and know how to write in RMarkdown / copy the resulting Knitted posts and figures to your Jekyll site etc. At some point I need to write a full start-to-finish guide to doing this: it's got a lot easier with some recent changes too. But for now, I just want to look at how to make the process smoother by automating some of the faff.

It might be things have moved on and there's some much easier way of doing this internal to RStudio - do let me know if so!

The KnitPost function above creates the md file and also puts any figures in a folder in the local project. You then have to copy these to the correct place in your Jekyll folder - and also edit the header so it's Jekyll-ready. Once you've done all that, you can preview locally [using jekyll serve](https://jekyllrb.com/docs/usage/) before pushing to the interwebs (as I'm doing here on my github.io site).

Which is great - but if you make an edit in your RmD, you have to then go through that process all over again. Mucho Fafferoo. I always have to make minor edits after I've posted something, as I suspect most people do: you don't see a lot of errors or typos until you're previewing and you're bound to think of something you missed. Having to go through this faff each time? Harrumph.

So I've written an update to the above script that moves the files to the correct place once they've been created and replaces the header with the correct YML. This means you just need to write the thing, then call the KnitPost function, and can preview changes immediately. 

The full function is below - just edit the URL of the Jekyll website and the path to your local Jekyll files.

And rather than load the function each time or package it up, I've just stuck mine into my Rprofile.site script so it's loaded whenever RStudio runs. (I think I got that [from here](http://www.onthelambda.com/2014/09/17/fun-with-rprofile-and-customizing-r-startup/).) You'll find the RProfile.site script in the Program folder of the R installation, in the 'etc' folder. (E.g. the path to mine is 'C:\Program Files\R\R-3.3.0\etc'). Just copy/paste the function at the end of the script.

Then you can just write your RMarkdown script and run:


{% highlight r %}
KnitPost('nameOfTheFile.Rmd','This is my lovely Jekyll article that will go to the right folder along with the figures')
{% endhighlight %}

The only little thing to note: it relies on there being three dashes to mark where to replace the header. By default, this is what RStudio creates if you make an Rmd script from its file menu. In theory it should be easy to code it to detect any number over three. I'll leave that as an exercise for the reader...

Right, this has nicely reminded me how this whole Jekyll blog thing works. Now let's start getting some less meta topics up here. Bye for now!


{% highlight r %}
#~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#Jekyll knitpost function
#~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 
  #Input should be the name of the Rmd file (won't need path if it's in your project)
  KnitPost <- function(input, articleName = NULL) {
  
    #Adapted from 
	  #http://gtog.github.io/workflow/2013/06/12/rmarkdown-to-rbloggers/
    
    #Change to your site
    base.url <- c("http://danolner.github.io/")
    
    #Move the md file and figures here (to appropriate folders) after the knit process
    #Needs doing afterwards to avoid wrong path.
    myjekyllpath = 'C:/localpathtoyourjekyllsite'
    
    #Final article name
    #use filename if not included
    if(is.null(articleName)){
      articleName <- sub(".Rmd$", "", basename(input))
    } 
    
    #so you're not creating the fig folder every time
    ifelse(!dir.exists(file.path('figs')), dir.create(file.path('figs')), FALSE)
    
    require(knitr)
    opts_knit$set(base.url = base.url)
    fig.path <- paste0("figs/", sub(".Rmd$", "", basename(input)), "/")
    opts_chunk$set(fig.path = fig.path)
    opts_chunk$set(fig.cap = "center")
    render_jekyll()
    knit(input, envir = parent.frame())
    
    #~~~~~~~~~~~~~~~~~~~~
    #Make edits and moves----
    #Make matching folder if doesn't exist (we may be overwriting files but not the folder)
    targetdir <- c(paste0(myjekyllpath,'/figs/',sub(".Rmd$", "", basename(input))))
    
    ifelse(!dir.exists(file.path(targetdir)), 
           dir.create(file.path(targetdir)),FALSE)
    
    #http://stackoverflow.com/questions/10299712/copying-list-of-files-from-one-folder-to-other-in-r
    filestocopy <- list.files(path = paste0("figs/", sub(".Rmd$", "", basename(input)), "/"),
                              full.names = T)
    
    #Move figure outputs to jekyll folder
    file.copy(from=filestocopy, to=targetdir, 
              overwrite = T, recursive = FALSE, 
              copy.mode = TRUE)
    
    #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    #Edit .md file to have jekyll-ready header----
    md <- readLines(paste0(sub(".Rmd$", ".md", basename(input))))
    
    #remove first line
    md <- md[2:length(md)]
    
    current <- md[1]
    
    while (current!='---') {
      #slice off the top
      md <- md[2:length(md)]
      current <- md[1]
      
    }
    
    #Then remove that line too once while done
    #(No do-while)
    md <- md[2:length(md)]
    
    #Add in jekyll header. This kinda structure
    # ---
    # layout: post
    # title: "testing again"
    # date: 2016-11-25
    # comments: true
    # ---
    theDate <- format(Sys.time(), "%Y-%m-%d")
    
    newHeader <- c(
      '---',
      'layout: post',
      paste0("title: \"", articleName,"\""),#this does work despite the output putting the escapes back in.
      paste0('date: ',theDate),#see ?date last example
      'comments: true',
      '---'
    )
    
    md <- c(newHeader,md)
    
    mdFileName <- paste0(myjekyllpath,
                         '/_posts/',
                         theDate,
                         '-',
                         gsub(' ','_',articleName),
                         '.md')
    
    #Ready to add to Jekyll folder
    writeLines(md,mdFileName)
    
  }
{% endhighlight %}













