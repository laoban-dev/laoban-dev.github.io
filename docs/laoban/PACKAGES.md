# Projects

A laoban project is a directory with a `package.details.json` file in it. This project MUST be under a directory that
has a [`laoban.json`](LAOBAN.JSON.md) file in it.

## Typical `package.details.json`

```json
{
  "template":    "typescript",
  "name":        "laoban",
  "description": "A cli for managing projects that have many npm packages",
  "links":       [
    "@laoban/variables", "@laoban/generations", "@laoban/validation", "@laoban/debug", "@laoban/files"
  ],
  "packageJson": {
    "dependencies": {
      "xlsx": "^0.18.5"
    },
    "devDependencies": {}
  },
  "guards":      {
    "compile": true,
    "test":    true,
    "publish": true
  }
}
```

| Name                           | Purpose                                                                                      |
|--------------------------------|----------------------------------------------------------------------------------------------|
| [template](TEMPLATES.md)       | This examples 'what type of project this is'.                                                |
| name                           | The name of the project. This is the value that the project would be published to in `npmjs` |
| description                    | Human readable description of the project.                                                   |
| [details.links](#dependencies) | This is how we express 'This project depends on other projects'                              |
| details.compile/test/publish   | These control if commands are executed in this project <br/>                                     |
| packageJson.dependencies       | These are the dependencies that will be added to the `package.json` file                      |
| packageJson.devDependencies    | These are the devDependencies that will be added to the `package.json` file  

## Project details file and variables

All the fields in a `package.details.json` file are available as [variables](VARIABLES.md) in scripts.

<a id='projects'></a>

## How do I find what packages exist

```shell
laoban packages
```

If we examine the results of this in the `laoban` project itself

```text
modules\debug       => @laoban/debug       (remoteTypescript)
modules\files       => @laoban/files       (typescript      ) depends on [@laoban/utils]
modules\generations => @laoban/generations (typescript      ) depends on [@laoban/debug,@laoban/utils]
modules\laoban      => laoban                 (typescript      ) depends on [@laoban/variables,@laoban/generations,@laoban/validation,@laoban/debug,@laoban/
files]
modules\utils       => @laoban/utils       (typescript      )
modules\validation  => @laoban/validation  (typescript      )
modules\variables   => @laoban/variables   (typescript      )
```

Each line corresponds to a packages. We can see the directory and the npm name of the packages.
The value in (brackets) is the [template](TEMPLATES.md) of the packages
The dependencies between the packages are shown at the end.

<a id='dependencies'></a>

## What are package dependencies?

Each `laoban package` can depend on others. This is important for things like 'compilation order'. If we are compiling
all the projects we want to compile the dependent projects first.

Dependencies are specified in the `package.details.json` file. A sample is shown here

Note that the links reference the `name` of the project not the directory. With a javascript project this is the
name that would be present in [npmjs](https://www.npmjs.com).

## How can I use the packages dependencies

You can execute any script in `link order` by

```shell
laoban helloWorld -l
```

## How can I see what order the commands will be executed in

Adding the `-g` option says `display the generation plan`. This will show the order that the

```shell
laoban helloWorld -lg
```

```text
Generation 0 modules\debug, modules\utils

Generation 1 modules\files, modules\generations, modules\variables, modules\validation

Generation 2 modules\laoban
```

This is telling us that all the projects in generation 0 will be executed before the projects in generation 1 and so on.

## How do I make a packages from scratch

* Create a subdirectory under the directory that holds the `laoban.json` file.
* Add to a file called `packages.details.json` (I usually copy an existing one)
* Decide what template it should be
* Make sure that the name and description are filled in correctly
* Add in any `links` if the project depends on another packages
* Execute `laoban update`

You should be able to see the packages now if you execute `laoban packages`

## Typical `package.details.json`

* `template` is the name of the subdirectory that holds the configuration files that laoban will place in the project
* `name` is the name of the project. This is injected into package.json by update
* `description` is the name of the project. This is injected into package.json by update
* `packageJson` holds information that will be merged into the package.json file
  * `dependencies` and `devDependencies` are the most common things to be added here
  * NOTE: you should not add other packages in the monorepo here. These are added in the `links` section
* `links` are used within the 'master project' that laoban is looking after. * It allows laoban to set up
  symbolic links so that changes in one project are immediately reflected 
  * These are added as dependencies to the project, with the 'current version number'
* `guards` are used to control what commands are executed in this package
  * `compile` Should this project be compiled with the typescript compiler
  * `test` does this project contain any tests
  * `publish` should this project be published


## Status

As described in [Scripts](SCRIPTS.md#complexCommands) some commands set `status` to true,
which means that the success or failure of the last run will be remembered and displayed
when you run `laoban status`.

Examples of this are `laoban compile`, `laoban test` and `laoban publish`.

## Profiling

For general interest the times of the `status` scripts is recorded and
can be seen with `laoban profile`. Each package is shown twice: once in `average`
and once in `latest`.

