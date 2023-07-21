# Sample repo to demonstrate upload times

## Summary

The GitHub CI/CD action `actions/upload-artifact@v3` runs *very* slowly when uploading build products to the SFU GitHub server. The upload time was measured for different sized files (1M to 100M) and uploaded to both `github.sfu.ca` and `github.com`. For example, the 100M file uploads to SFU GitHub between 300 and 700 seconds (~1-3 MBits/second), whereas the same size file uploads to `github.com` in less than 50 seconds (~16 MBits/second).

The slowness of the upload to SFU GitHub is a serious issue because it can add up to half an hour to the pipeline build for some projects.

## Details

This project contains a `.github\workflows` folder which defines a CI/CD pipeline that:

1. Creates a big file (size 1M to 100M) and uploads it to GitHub as a build product.
2. Downloads the build products to another job.

This repo is setup with an SFU hosted VM to run the CI/CD pipeline build steps. The same pipeline configuration and GitHub runner were used upload files to the SFU GitHub server, and the public GitHub.com server. It was found that upload to SFU was about 8 to 16 times slower than the public github. See the full `DemoSlowUploads-Results.ods` file for details and a graph.

The tests were re-run using a matrix of different size options. See the `DemoSlowUploads-Results.ods` file for a table of values and graph showing the speed. I note that there are some interesting non-linearities in the data for which I have no explanation; however, the real concern is how slow it is. **Overall the test runs, the upload runs at about 1 to 3 Mbits/s.**

## Investigation

* It's likely not a network speed issue (on the VM side at least) because running the `speedtest-cli --secure` command says download is 934 Mbits/s, and upload is 805 Mbits/s (to FIBRETEL, Vancouver BC).
* Since the VM is able to upload at ~18MBits/s to `github.com`, and only 1-3MBits/s to `github.sfu.ca`, it is likely a problem with the SFU GitHub rather than either the VM running the GitHub runner, or the network connecting to the VM.
* Since the VM is able to download large files from the SFU GitHub server quickly, it is likely not a network issue.

