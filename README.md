# Safer GitHub Actions Workflow for golangci-lint

This workflow downloads, verifies, and runs [golangci/golangci-lint](https://github.com/golangci/golangci-lint) in a deterministic, easy to audit, and safe manner.

I'm not affiliated with golangci-lint project. You can sponsor them at: https://github.com/sponsors/golangci.

## Why use safer-golangci-lint.yml?

Use this to avoid two problems:
- Bad practice of using curl-to-shell, which allows 3rd-party script to change without notice and execute.
- More time-consuming practice of reviewing and pinning [golangci-lint-action](https://github.com/golangci/golangci-lint-action) requires:
  - Reviewing each release of golangci-lint-action, which is relatively time-consuming.
  - Pinning each release after reviewing the code.

With safer-gbolangci-lint.yml, you only need to update 2 variables when you want to use a newer golangci-lint:
 - Version number of golangci-lint.  E.g. "1.56.2".
 - SHA-256 digest of golangci-lint-X.XX.X-linux-amd64.tar.gz. E.g. where "X.XX.X" is "1.56.2".

What you download and execute shouldn't be able to change at anytime without your review. Linting the same source code should produce deterministic results unless you choose to modify the workflow or linter settings.

Directly executing the output of curl, etc. without verifying the integrity of the downloaded script is risky.  The [Codecov bash script incident](https://www.securityweek.com/codecov-bash-uploader-dev-tool-compromised-supply-chain-hack) is an example of what can happen.

Everything you need to download, verify, and execute a verified file should be self-contained in your workflow and under your control.

Projects using safer-golangci-lint.yml:
 - [fxamacker/cbor](https://github.com/fxamacker/cbor)
 - [fxamacker/circlehash](https://github.com/fxamacker/circlehash)
 - [x448/float16](https://github.com/x448/float16)

## safer-golangci-lint.yml

100% of the script for downloading, verifying, and running golangci-lint is embedded in the workflow file.  You can review it once and continuously use it knowing it won't be changed by a 3rd party unless you update the script yourself in your repo.

The embedded SHA-256 digest is used to verify the downloaded golangci-lint tarball (e.g. golangci-lint-1.56.2-linux-amd64.tar.gz). It matches the official checksums file at https://github.com/golangci/golangci-lint/releases.

More specifically, this workflow:

1. Avoids downloading and executing unverified wrapper scripts.  
   See https://www.securityweek.com/codecov-bash-uploader-dev-tool-compromised-supply-chain-hack
2. Uses openssl instead of sha256sum because it's easier to change hash algo to BLAKE2s, SHA3-256, etc.
3. Uses embedded SHA-256 because CHECKSUM file isn't digitally signed.  The embedded SHA-256 matches the official CHECKSUM file and it cannot change without you change safer-golangci-lint.yml in your repo.

## Quick Start
Step 1. Copy [safer-golangci-lint.yml](https://raw.githubusercontent.com/x448/safer-golangci-lint/main/safer-golangci-lint.yml) into [github_repo]/.github/workflows/  
Step 2. There's no step 2 if you like the default settings.

You can use a config file (.golangci.yml) as described by the [golangci-lint](https://github.com/golangci/golangci-lint) project.

## Upgrading to newer version of golangci-lint

Just copy the latest version of safer-golangci-lint.yml into your .github/workflows folder.

Or if you want to manually update, you can:

1. specify new version numbers for `GO_VERSION` and `GOLINTERS_VERSION`
2. specify new hash of tarball for `GOLINTERS_TGZ_HASH`

![image](https://github.com/user-attachments/assets/f0bce474-974a-44c4-bf3a-5c3edc7c9150)

## Release v1.56.2

Changes:
- Bump golangci-lint to 1.56.2
- Bump go to 1.22
- Bump actions/checkout to 4.1.7
- Bump actions/setup-go to 5.0.2
- Hash of golangci-lint-1.56.2-linux-amd64.tar.gz
  SHA-256: e1c313fb5fc85a33890fdee5dbb1777d1f5829c84d655a47a55688f3aad5e501  

SHA-256
- safer-golangci-lint.yml (v1.56.2): 0f1741d505320176c140b311c0b7e942bc4c438428eda47555409a104502aecf
- golangci-lint-1.56.2-linux-amd64.tar.gz: e1c313fb5fc85a33890fdee5dbb1777d1f5829c84d655a47a55688f3aad5e501  
  This SHA-256 digest matches [golangci-lint-1.56.2-checksums.txt](https://github.com/golangci/golangci-lint/releases/download/v1.56.2/golangci-lint-1.56.2-checksums.txt) at
  https://github.com/golangci/golangci-lint/releases

## License

safer-golangci-lint is licensed under MIT License.  See [LICENSE](LICENSE) for the full license text.  
https://github.com/x448/safer-golangci-lint

Copyright © 2021-2024 Montgomery Edwards⁴⁴⁸ (github.com/x448).
