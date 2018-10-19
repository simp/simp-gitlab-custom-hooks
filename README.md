# SIMP Custom Gitlab Hooks

This repository contains custom GitLab hooks that can be used for various
configurations and workflow modifications.

## Global hook configuration

The repository uses a global yaml hook configuration file. This can be used to
configure various settings for all of the hooks used on the server side and to
load these settings dynamically.

This file should be saved to /path/to/repository.git/custom_hooks/hooks.yaml

## Installing hooks

For server-side custom hooks (pre-receive, update, post-receive), drop the hook
script into /path/to/repository.git/custom_hooks/{pre-receive,update,post-receive}
on the server itself.

Drop the hook configuration into /path/to/repository.git/custom_hooks/hooks.yaml

## Pre-receive hooks

### Branch-based Merge Hook

This hook evaluates a Merge Request to ensure that a merge to a controlled branch
is from the head commit of a valid/whitelisted branch. For example: master can
only be merged to from staging

#### Configuring branches

In the hooks.yaml file branches can be specified under

```yaml
pre-receive:
  branch_based_merge:
    branches:
```

This is expected to be a hash value where the keys are the branches that are
controlled and each key has an array of whitelisted branches. So in the example
above, plus a 'production' branch being able to be merged from 'master' the
configuration would look as follows:

```yaml
pre-receive:
  branch_based_merge:
    branches:
      master:
        - 'staging'
      production:
        - 'master'
```

Note: If a branch is not listed in the branches list, there will be no restrictions
on what other branches can be merged. Also, reverts (which essentially are pushed
as merges are also allowed).

#### Out-of-band merges

Sometimes, merges out-of-band need to occur; as in the example of a breaking bug
fix or a security update. To allow this, a regex can be set to match the merge
request description against. To enable this, set both the `oob_regex` and the
`api_token` settings within the configuration file.

The `api_token` value is a valid GitLab API token that has access to the project
the pre-receive hook is configured for. This is required to allow access to the
Merge Request GitLab API to pull the merge request data.
