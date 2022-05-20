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
downloaded golangci-lint tarball (e.g. golangci-lint-1.42.0-linux-amd64.tar.gz). 

## Why?
1. Avoid downloading and executing unverified wrapper scripts or actions each time a workflow runs.  
   See https://www.securityweek.com/codecov-bash-uploader-dev-tool-compromised-supply-chain-hack
2. Use openssl instead of sha256sum because it's easier to change hash algo to BLAKE2s, SHA3-256, etc.
3. Use embedded SHA384 instead of downloading CHECKSUM because CHECKSUM file isn't digitally signed.
4. Use binary instead of building from source because it's probably easier to detect backdoors in one binary 
   than all the combined source code of dozens of linters and all their required 3rd-party packages.

## Release 1.46.2 (May 19, 2022)
Changes:
  - Bump golangci-lint to 1.46.2.
  - Remove default permissions at top level and grant only read permission in the job.
  - actions/setup-go uses check-latest: true
  - Add workflow_dispatch.
  - Tidy some comments.
  - SHA-256(golangci-lint-1.42.0-linux-amd64.tar.gz) is 242cd4f2d6ac0556e315192e8555784d13da5d1874e51304711570769c4f2b9b
  - SHA-384(golangci-lint-1.42.0-linux-amd64.tar.gz) is 60ade95e447f8c9a2dfc507c271c2ff41a0e0856f077bf2f734bcd80dd8268addf8cf1625c3e47a6516eb14f23423315

SHA-384 of safer-golangci-lint.yml (v1.46.2) is  
61b3abc35547b1bc662fec29e3bd72c294af0dee618dbbf6e70032bf2ee38487b9adab9793770f3405fec0da81f57733

## License
safer-golangci-lint is licensed under MIT License.  See [LICENSE](LICENSE) for the full license text.  
https://github.com/x448/safer-golangci-lint

Copyright © 2021 Montgomery Edwards⁴⁴⁸ (github.com/x448).
