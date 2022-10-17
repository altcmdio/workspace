## Workspace

Minimal template repository I use with [`degit`](https://www.npmjs.com/package/tiged) to kick-start mono-repo projects backed by [Yarn Workspaces](https://yarnpkg.com/features/workspaces). Creates a consistent [development container](https://code.visualstudio.com/docs/remote/containers-tutorial) based environment.

## Quick start

```sh
# globally install `degit` command, if not already installed
# npm install -g tiged

degit altcmdio/workspace my-project
cd my-project
git init . # degit creates a bare copy of this repo
code -n .

```
