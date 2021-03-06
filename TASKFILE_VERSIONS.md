# Taskfile version

The Taskfile syntax and features changed with time. This document explains what
changed on each version and how to upgrade your Taskfile.

# What the Taskfile version mean

The Taskfile version follows the Task version. E.g. the change to Taskfile
version `2` means that Task `v2.0.0` should be release to support it.

The `version:` key on Taskfile accepts a semver string, so either `2`, `2.0` or
`2.0.0` is accepted. You you choose to use `2.0` Task will not enable future
`2.1` features, but if you choose to use `2`, than any `2.x.x` features will be
available, but not `3.0.0+`.

## Version 1

In the first version of the `Taskfile`, the `version:` key was not available,
because the tasks was in the root of the YAML document. Like this:

```yml
echo:
  cmds:
    - echo "Hello, World!"
```

The variable priority order was also different:

1. Call variables
2. Environment
3. Task variables
4. `Taskvars.yml` variables

## Version 2.0

At version 2, we introduced the `version:` key, to allow us to envolve Task
with new features without breaking existing Taskfiles. The new syntax is as
follows:

```yml
version: '2'

tasks:
  echo:
    cmds:
      - echo "Hello, World!"
```

Version 2 allows you to write global variables directly in the Taskfile,
if you don't want to create a `Taskvars.yml`:

```yml
version: '2'

vars:
  GREETING: Hello, World!

tasks:
  greet:
    cmds:
      - echo "{{.GREETING}}"
```

The variable priority order changed to the following:

1. Task variables
2. Call variables
3. Taskfile variables
4. Taskvars file variables
5. Environment variables

A new global option was added to configure the number of variables expansions
(which default to 2):

```yml
version: '2'

expansions: 3

vars:
  FOO: foo
  BAR: bar
  BAZ: baz
  FOOBAR: "{{.FOO}}{{.BAR}}"
  FOOBARBAZ: "{{.FOOBAR}}{{.BAZ}}"

tasks:
  default:
    cmds:
      - echo "{{.FOOBARBAZ}}"
```
