# Safer GitHub Actions Workflow for golangci-lint

Use golangci-lint more securely in your CI workflow.

## Quick Start
Copy safer-golangci-lint.yml into [your_github_repo]/.github/workflows/

You should configure your .golangci.yml normally, as instructed in golangci-lint docs.

## Upgrading to newer version of golangci-lint
1. specify new version number in GOLINTERS_VERSION
2. specify new hash of tarball in GOLINTERS_TGZ_HASH

## safer-golangci-lint.yml

100% of the script for downloading, installing, and running golangci-lint
is embedded in the workflow file.  The embedded SHA384 is used to verify the 
downloaded golangci-lint tarball (e.g. golangci-lint-1.40.1-linux-amd64.tar.gz). 

## Why?
1. Avoid downloading and executing unverified wrapper scripts or actions each time a workflow runs.  
   See https://www.securityweek.com/codecov-bash-uploader-dev-tool-compromised-supply-chain-hack
2. Use openssl instead of sha256sum because it's easier to change hash algo to BLAKE2s, SHA3-256, etc.
3. Use SHA384 instead of SHA256 to avoid debating strangers about length extension attacks and gzip file format.
4. Use embedded SHA384 instead of downloading CHECKSUM because CHECKSUM file isn't digitally signed.
5. Use binary instead of building from source because it's probably easier to detect backdoors in one binary 
   than all the combined source code of dozens of linters and all their required 3rd-party packages.

## Changelog
2021-05-16  Created. Use golangci-lint 1.40.1, Go 1.15.x, and ubuntu-latest.  
 - SHA256 of golangci-lint-1.40.1-linux-amd64.tar.gz is  
   7c133b4b39c0a46cf8d67265da651f169079d137ae71aee9b5934e2281bd18d3
 - SHA384 of golangci-lint-1.40.1-linux-amd64.tar.gz) is  
   d0b9e9c0eac5c5e03b9feb546d181918fca9abc94656824badccacc77aa91bc78ab99fd22094d634d3a58a91353fb1b9

## License
safer-golangci-lint is licensed under MIT License.  See [LICENSE](LICENSE) for the full license text.  
https://github.com/x448/safer-golangci-lint

Copyright © 2021 Montgomery Edwards⁴⁴⁸ (github.com/x448).
