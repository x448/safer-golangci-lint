# Safer GitHub Actions Workflow for golangci-lint

Use [golangci/golangci-lint](https://github.com/golangci/golangci-lint) more securely in your CI workflow.

Projects using this workflow file includes:
 - [fxamacker/cbor](https://github.com/fxamacker/cbor)
 - [x448/float16](https://github.com/x448/float16)

## Quick Start
Step 1. Copy [safer-golangci-lint.yml](https://raw.githubusercontent.com/x448/safer-golangci-lint/main/safer-golangci-lint.yml) into [github_repo]/.github/workflows/  
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
3. Use embedded SHA384 instead of downloading CHECKSUM because CHECKSUM file isn't digitally signed.
4. Use binary instead of building from source because it's probably easier to detect backdoors in one binary 
   than all the combined source code of dozens of linters and all their required 3rd-party packages.

## Release 1.42.0 (August 22, 2021)
Changes:  
  - Bump golangci-lint to 1.42.0.
  - SHA256(golangci-lint-1.41.1-linux-amd64.tar.gz) is 6937f62f8e2329e94822dc11c10b871ace5557ae1fcc4ee2f9980cd6aecbc159
  - SHA384(golangci-lint-1.41.1-linux-amd64.tar.gz) is 6bc3704f79a14150467f651e4f7a3975caf555fc93fe5fae7aa3b1d5f7eaf7261d97486298d169b241b587f52e46025e

SHA384(safer-golangci-lint.yml) v1.42.0 is  
9960c18d76df37c9f7ee562d608daef44dc24c642b8ffd72917b2b4a92ff74c5707e16f684d94b19eceed5c11bb463a1

## License
safer-golangci-lint is licensed under MIT License.  See [LICENSE](LICENSE) for the full license text.  
https://github.com/x448/safer-golangci-lint

Copyright © 2021 Montgomery Edwards⁴⁴⁸ (github.com/x448).
