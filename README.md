# Scoop Bucket

<!-- Uncomment the following line after replacing placeholders -->
[![Tests](https://github.com/renxia/scoop-bucket/actions/workflows/ci.yml/badge.svg)](https://github.com/renxia/scoop-bucket/actions/workflows/ci.yml) [![Excavator](https://github.com/renxia/scoop-bucket/actions/workflows/excavator.yml/badge.svg)](https://github.com/renxia/scoop-bucket/actions/workflows/excavator.yml)

A personal bucket for [Scoop](https://scoop.sh), the Windows command-line installer.

## Setup Scoop

```bash
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
irm https://gh-proxy.org/raw.githubusercontent.com/lzwme/scoop-proxy-cn/master/install.ps1 | iex

scoop bucket add spc https://gh-proxy.org/github.com/lzwme/scoop-proxy-cn
```

大陆用户请参阅： [https://github.com/lzwme/scoop-proxy-cn](https://github.com/lzwme/scoop-proxy-cn)

## How do I install these manifests?

After manifests have been committed and pushed, run the following:

```pwsh
scoop bucket add rsb https://github.com/renxia/scoop-bucket
```

## How do I contribute new manifests?

To make a new manifest contribution, please read the [Contributing
Guide](https://github.com/ScoopInstaller/.github/blob/main/.github/CONTRIBUTING.md)
and [App Manifests](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests)
wiki page.
