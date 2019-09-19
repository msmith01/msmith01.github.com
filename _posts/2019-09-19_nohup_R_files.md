---
layout: post
title: Sending nohup commands to R scripts
subtitle: A breif discussion on nohup commands for R scripts
gh-repo: https://github.com/msmith01
gh-badge: [star, fork, follow]
tags: [AWS, RStudio]
comments: true
---


There are times when I am connected remotely to an RStudio server and I want to run a time-consuming script which might take a few hours or days. As part of my PhD I had to calculate a number of large distance matrices which took considerable time since the data was quite large. Another script I developed was to download and parse 150,000 documents. It took a week to download and parse them and during this time I lost connection to the server a few times causing the code to break.

The free version of RStudio only allows one session open at any time and leaving the script running for a week meant that I couldn’t keep coding in R! Many of the problems I faced actually didn’t require RStudio since the models were either downloading and storing data or calculating matrices so just an R connection would be sufficient. So, I wanted to find a way to run an R file without logging off or without any interruption. I come across the **nohup** command in Unix operating systems.

The **nohup** or **no hang-up** command can execute a script and tell the system to continue running it even if the session is disconnected.
I use it in the following way, once I have developed a model *myRFile.R* using RStudio, verified it works on a sample of the data etc. and saved it into a folder called *myFolder*. I can go to the terminal (which I usually connect to using PuTTY) and run the following:

nohup Rscript ./myFolder/myRFile.R </dev/null>myRFile_Output.out 2>myRFile_ErrorMessages.err &

This will run the .R file and store any output usually printed to the console to a text file *myRFile_Ouput.out* and any error messages to another text file *myRFile_ErrorMessages.err* where we can check on the progress whilst the script is running. I usually add progress messages in side the function such as:

```R
foo <- function(x){
  print(paste("Year", x, "has started ", Sys.time()))
  
  do.something
  
  print(paste("Year", x, "has ended ", Sys.time()))
  return(x)
}

years <- seq(from = "2005", to = "2018")
sapply(years, foo)  
```
Which prints out the progress of the function, i.e. if I needed to apply a function across years.

By running *nohup* commands from the terminal allows me to free up my RStudio for other tasks also. 
