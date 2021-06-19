# Safer GitHub Actions Workflow for golangci-lint

Use golangci-lint more securely in your CI workflow.

Projects using this workflow file includes:
 - [fxamacker/cbor](https://github.com/fxamacker/cbor)
 - [x448/float16](https://github.com/x448/float16)

## Quick Start
Step 1. Copy safer-golangci-lint.yml into [github_repo]/.github/workflows/  
Step 2. There's no step 2 if you like the default settings.

You can use a config file (.golangci.yml) as described by the [golangci-lint](https://github.com/golangci/golangci-lint) project.

## Upgrading to newer version of golangci-lint than this project
1. specify new version number in GOLINTERS_VERSION
2. specify new hash of tarball in GOLINTERS_TGZ_HASH

## safer-golangci-lint.yml

100% of the script for downloading, installing, and running golangci-lint
is embedded in the workflow file.  The embedded SHA384 is used to verify the 
downloaded golangci-lint tarball (e.g. golangci-lint-1.41.1-linux-amd64.tar.gz). 

## Why?
1. Avoid downloading and executing unverified wrapper scripts or actions each time a workflow runs.  
   See https://www.securityweek.com/codecov-bash-uploader-dev-tool-compromised-supply-chain-hack
2. Use openssl instead of sha256sum because it's easier to change hash algo to BLAKE2s, SHA3-256, etc.
3. Use SHA384 instead of SHA256 to avoid debates about length extension attacks and gzip file format.
4. Use embedded SHA384 instead of downloading CHECKSUM because CHECKSUM file isn't digitally signed.
5. Use binary instead of building from source because it's probably easier to detect backdoors in one binary 
   than all the combined source code of dozens of linters and all their required 3rd-party packages.

## Release 1.14.1 (June 19, 2021)
Changes:  
  - Bump Go to 1.16.x and golangci-lint to 1.41.1.
  - Increase default timeout to 5 minutes.
  - Remove optional noisy run because "noisy" is too subjective.
  - SHA256(golangci-lint-1.41.1-linux-amd64.tar.gz) is 23e1078ab00a750afcde7e7eb5aab8e908ef18bee5486eeaa2d52ee57d178580
  - SHA384(golangci-lint-1.41.1-linux-amd64.tar.gz) is 8e966704696875f39d324a2f321ac1f63edab08668d8e09fa06dbc54ffe4c4bf4796c80d611d7b40ca42a4b33c208800

SHA384(safer-golangci-lint.yml) v1.14.1 is  
e36dbdfad2ee209be0172e1598d00e74f51dd5eaff3e896c525e6672304f2ee15fd7e4739753f7f352868539e7e7241f

## License
safer-golangci-lint is licensed under MIT License.  See [LICENSE](LICENSE) for the full license text.  
https://github.com/x448/safer-golangci-lint

Copyright © 2021 Montgomery Edwards⁴⁴⁸ (github.com/x448).
