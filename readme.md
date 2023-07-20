# Sample repo to demonstrate upload times

The GitHub CI/CD action `actions/upload-artifact@v3` runs *very* slowly on the SFU GitHub server.

This project contains a `.github\workflows` folder which defines a CI/CD pipeline that:

1. Creates a big file and uploads it to GitHub as a build product.
2. Downloads the build products to another job.

This repo is setup with an SFU hosted VM to run the CI/CD pipeline build steps. When run on SFU's GitHub server, the times are:

Upload 1M: 
Download 1M: