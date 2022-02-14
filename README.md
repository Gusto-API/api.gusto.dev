# Gusto Partner API Documentation

## Contibuting (internal)

### Setup

We use [spectral](https://meta.stoplight.io/docs/spectral/ZG9jOjYx-overview) in a pre-commit hook, so you need to run `yarn install` after cloning this repo.

```shell
git clone git@github.com:Gusto-API/api.gusto.dev.git
yarn
```

### Change request

The DevRel team track changes to the api on their Jira board if you want to request a change.

### Change approval

If you want to do the change yourself, have your changes go through the DevRel team first for approval

### How to

The documentation is generated through [stoplight.io](gusto.stoplight.io).

While you could update this repo directly through the Github UI interface, it is not recommended. There is currently no CI check for this repo and a syntax error could take down the docs.

The recommended way to do it is through stoplight.io. You can still create new branches/PR through stoplight.io.

Here is the process: https://docs.google.com/document/d/1KxoLOgQLf8s3p0A5_qZCiJ_Kf0EreS18UeJfiNFqMrw

We also have a process for editing directly this repo (not recommended): https://docs.google.com/document/d/1H4vtaynEvc74ZXfMb-ED34I5Ur4QLPM_ZkRvJ8dENp4

### Auth

You need write access on both https://gusto.stoplight.io and the https://github.com/Gusto-API/api.gusto.dev repo.

You can ask for access in the PIE Hire channel: #pie-hire
