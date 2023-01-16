# Installing

* `npm i -g laoban@latest` will install `laoban`

# Laoban Cheatsheet

| Command | Guarded? | Purpose |
| --- | --- | --- |
| `laoban update` | No | Makes sure that the packages are in line with the templates
| `laoban update --minor` | No |Updates the version number of the packages. Usually before doing a `laoban publish`
| `laoban update --major` | No |Updates the version number of the packages for a major release. Usually before doing a `laoban publish`
| `laoban clean` | No | cleans up the directories and files that laoban uses as temporary storage
| `laoban compile -asl` | details.compile | Compile all the typescript projects in the correct order
| `laoban test -asl` | details.test | Test all the typescript projects in the correct order
| `laoban publish -asl` | details.publish | Publishes the projects to npm
| `laoban status -a` | No |  Show the status of important commands (compile/test) across all projects
| `laoban run 'rm -rf node_modules'` | No | In linux remove all the node modules from the project (be careful)

# Adding a new `package` to an existing `laoban` project

| Command  |  Purpose |
| --- | --- |
| `laoban admin newpackage <directory>` |  the specified directory is created if needed and turned into a `package`. Options can be used to control the template and the package name
| `laoban admin newpackage` |  the current directory is turned into a `package`. Options can be used to control the template and the package name。 Or you can just inspect and edit the `package.details.json` file afterwards (and run `laoban update` after editing)
| `laoban admin newpackage  -p <packageName>` |  the current directory is turned into a `package`. The name of the package is set to `<packageName>`
| `laoban admin newpackage  --template <template>` |  the current directory is turned into a `package`. The template of the package is set to `<template>`
| `laoban admin templates` |  shows the available templates usable in the --template option above

## Notes

* If the `package.details.json` already exists (i.e. it is already a new package) then you will need to add `--force` to overwrite it.
* If the template you want to use isn't in the list give by `laoban admin templates` then you need to modify the `laoban.json` file to add it

# laoban admin Cheatsheet

[laoban-admin](https://www.npmjs.com/package/@laoban/admin) is the recommended way
to [getting started](GETTING.STARTED.md) with existing projects

It also holds commands for rarely used admin commands like 'clearcache' and 'profile'

| Command |  Purpose |
| --- | --- |
| `laoban admin projects` | Lists the found projects in the monorepo (with package.json files) and what is the recommended template
| `laoban admin init --dryrun` |  Generates 'test' files that allow you to look at them before using them
| `laoban admin init --force` |  Sets up your project with `laoban.json` and `package.details.json` that should work
| `laoban admin newpackage` |  the current directory is turned into a `package`. Options can be used to control the template and the package name
| `laoban admin newpackage  -p <packageName>` |  Creates a new package in the monorepo
| `laoban admin newtemplate`  |  creates a new template with a default name from the files in the current directory placing the template under the `templates` directory (under the current directory)
| `laoban admin newtemplate --directory <directory>`  |  creates a new template with a default name from the files in the `directory` placing the template under the `templates` directory (under the current directory)
| `laoban admin newtemplate -t <templatename> --directory <directory> --template <templatesdirectory> ` |  creates a new template called `<templatename>` from the files in the `<directory>`, placing the template under the `templatesdirectory` directory
| `laoban admin makeintotemplate --directory <directory> ` |  Turns the <directory> into a template
| `laoban admin clearcache` |  Used to clear the cache of the files loaded from urls. Unless they have changed there is little value in this
| `laoban admin profile` |  Show the time taken to do important tasks across all packages 





