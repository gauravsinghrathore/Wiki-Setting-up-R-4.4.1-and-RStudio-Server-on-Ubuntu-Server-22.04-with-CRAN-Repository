# Wiki-Setting-up-R-4.4.1-and-RStudio-Server-on-Ubuntu-Server-22.04-with-CRAN-Repository

## We needed to install R on an Ubuntu 22.04 server (c8pu01) and configure RStudio Server to work with that R version. The default R versions in Ubuntu repositories are outdated, so we decided to use CRAN’s Ubuntu repository for newer R versions. However, some network restrictions (like SSH bastion proxies) initially blocked direct access to CRAN.
