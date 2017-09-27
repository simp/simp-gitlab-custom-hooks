# SIMP Custom Gitlab Hooks

This repository contains custom GitLab hooks that can be used for various
configurations and workflow modifications.

## Installing hooks

For server-side custom hooks (pre-receive, update, post-receive), drop the hook
script into /path/to/repository.git/custom_hooks/{pre-receive,update,post-receive}
on the server itself.
