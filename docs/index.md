--- 
title: "Supplementary Appendix for Beyond the Wire: US Military Deployments and Host Country Public Opinion"
author: "Michael. A. Allen, Michael E. Flynn, Carla Martinez Machain, and Andrew Stravers"
date: "2022-11-03"
site: bookdown::bookdown_site
documentclass: book
cover-image:
bibliography: [one.bib]
# url: your book url like https://bookdown.org/yihui/bookdown
# cover-image: path to the social sharing image like images/cover.jpg
description: |
  This bookdown project contains additional supplementary information for the contents of our book, Beyond the Wire: US Military Deployments and Host Country Public Opinion. The materials contained within include additional information on surveys, tables for the models presented in the book, additional figures, etc.
link-citations: yes
github-repo: meflynn/beyond-the-wire-appendix
---

# Summary {-}

This bookdown project contains additional supplementary information for the contents of our book, Beyond the Wire: US Military Deployments and the Diplomacy of Defense. The materials contained within include additional information on surveys, tables for the models presented in the book, additional figures, code for figures and tables, etc.

In general, we try to annotate the code to help users understand the various decisions we made throughout. Given the length of the manuscript and the sheer volume of code a line-by-line annotation isn't currently possible, but we will will provide a generally summary of denser material and highlight particular lines at more critical points in the workflow. 

Also note that we try to load the appropriate libraries at the beginning of each appendix chapter, but some of these may be redundant or unused in the final iteration of the chapter. In many cases we also call the library as a part of the function call to ensure reproducability and avoid errors resulting from mistakenly calling a function from a different package (e.g. `{plyr::summarize}` rather than `{dplyr::summarize}`). 

This supplementary appendix generally follows the layout of the book and is comprised of separate chapters that contain information from the corresponding book chapter, with the exception of the theory chapter, which is largely lacking any data or accompanying code. The general layout is as follows:

1. Introduction
2. Domain of Consent
3. Deployments and Contact
4. Deployments and Crime
5. Deployments and Minority Populations
6. Deployments and Protests
7. Domain of Competitive Consent

All remaining errors are our own.
