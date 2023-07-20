# Sample repo to demonstrate upload times

The GitHub CI/CD action `actions/upload-artifact@v3` runs *very* slowly on the SFU GitHub server.

This project contains a `.github\workflows` folder which defines a CI/CD pipeline that:

1. Creates a big file and uploads it to GitHub as a build product.
2. Downloads the build products to another job.

This repo is setup with an SFU hosted VM to run the CI/CD pipeline build steps. When run on SFU's GitHub server, the times are:

* Upload 20M from a container: 2m15s
* Upload 20M without a container: 2m30s
* Download two 20M files: 13s

The tests were re-run using a matrix of different size options. See the `DemoSlowUploads-Results.ods` file for a table of values and graph showing the speed. I note that there are some interesting non-linearities in the data for which I have no explanation; however, the real concern is how slow it is overall. **Overall the test runs, the upload runs at about 5 Mbits/s.**

## Investigation

* It's likely not a network speed issue (on the VM side at least) because running the `speedtest-cli --secure` command says download is 934 Mbits/s, and upload is 805 Mbits/s (to FIBRETEL, Vancouver BC).
