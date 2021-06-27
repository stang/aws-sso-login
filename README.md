# aws-sso-login

`aws-sso-login` is a lightweight wrapper that relies on `aws-cli`'s built-in `aws sso login`.

It adds an extra steps that set the short-lived credentials directly in your `~/.aws/credentials` file.

That allow backward-compatibility with 3rd party tools that are not supporting yet the new credentials format.

## Context

When using an `AWS SSO`, users can retrieve short-lived access keys:
* from the _User Portal_ (ie: `https://xxx.awsapps.com/start`)
* or using [the `aws sso login` command](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html#sso-using-profile)

When using the `aws sso login` option, the short-lived credentials are stored in `~/.aws/cli/cache`.

Historically, credentials were rather stored in `~/.aws/credentials`.

Some 3rd party tools are still not supporting credentials from `~/.aws/cli/cache`.

## Prerequisites

* `aws-cli` v2
* `jq`

## Install

* copy the `aws-sso-login` in your `$PATH`
* make it executable

```
INSTALL_DIR=/usr/local/bin
sudo wget -O "${INSTALL_DIR}/aws-sso-login" https://raw.githubusercontent.com/stang/aws-sso-login/master/aws-sso-login
sudo chmod +x "${INSTALL_DIR}/aws-sso-login"
```

## Usage

1. [configure aws sso](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html#sso-configure-profile-auto)
2. use `aws-sso-login [profile]` instead of `aws sso login`

## Support multiple AWS Profile

`aws-sso-login` will use the AWS profile set as following (first match takes precedences):

- $1 (ie: first optional argument of the command)
- AWS_PROFILE environment variable
- AWS_SSO_DEFAULT_PROFILE environment variable
- use 'default'

## Known Issues

* We're sacrifying the `~/.aws/cli/cache` mechanism ([see details](https://github.com/stang/aws-sso-login/commit/b3d9c2c2ee9ec342e88bdc115afb884377bca015))
