# Installing

* `npm i -g laoban@latest` will install `laoban`

# Laoban Cheatsheet

| Command                                 | Guarded?        | Purpose                                                                                                 |
|-----------------------------------------|-----------------|---------------------------------------------------------------------------------------------------------|
| `laoban update`                         | No              | Makes sure that the packages are in line with the templates                                             |                                           
| `laoban update --minor`                 | No              | Updates the version number of the packages. Usually before doing a `laoban publish`                     |
| `laoban update --major`                 | No              | Updates the version number of the packages for a major release. Usually before doing a `laoban publish` |
| `laoban clean`                          | No              | cleans up the directories and files that laoban uses as temporary storage                               |
| `laoban compile`                        | details.compile | Compile all the typescript projects in the correct order                                                |            
| `laoban test`                           | details.test    | Test all the typescript projects in the correct order                                                   |        
| `laoban publish`                        | details.publish | Publishes the projects to npm (requires NPM_TOKEN environment variable setting)                         |          
| `laoban status`                         | No              | Show the status of important commands (compile/test) across all projects                                |
| `laoban run 'file:rmDir(node_modules)'` | No              | In linux remove all the node modules from the project (be careful)                                      |

# Adding a new `package` to an existing `laoban` project

| Command                                          | Purpose                                                                                                                                                                                                                                 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `laoban admin newpackage <directory>`            | the specified directory is created if needed and turned into a `package`. Options can be used to control the template and the package name                                                                                              |                                                                                              
| `laoban admin newpackage`                        | the current directory is turned into a `package`. Options can be used to control the template and the package name。 Or you can just inspect and edit the `package.details.json` file afterwards (and run `laoban update` after editing) | 
| `laoban admin newpackage  -p <packageName>`      | the current directory is turned into a `package`. The name of the package is set to `<packageName>`                                                                                                                                     |
| `laoban admin newpackage  --template <template>` | the current directory is turned into a `package`. The template of the package is set to `<template>`                                                                                                                                    |
| `laoban admin templates`                         | shows the available templates usable in the --template option above                                                                                                                                                                     |

* If the `package.details.json` already exists (i.e. it is already a new package) then you will need to add `--force` to
  overwrite it.
* If the template you want to use isn't in the list give by `laoban admin templates` then you need to modify
  the `laoban.json` file to add it

# Changing a dependency in a template

For example the version of typescript or the version of react

* Change the `package.json` in one package that uses the template: set the dependency to the version you want
* `laoban admin updateTemplate` running in the directory of the package will update the template with the new dependency
* `laoban update` will be needed afterwards to update the packages that use the template

# Adding dependencies to all packages using a specific template

* If the template is already in the local filesystem (look in the `templates` directory) then go
  edit the package.json in there. Note that the package.json is typically tiny only including 'extras'.
* If the template is not already in there then you need to add it first the instructions above
  in `Changing a dependency in a template`

# Make a new template from scratch

Usually we want to extend a template (for example we might just want to update a dependency as above)

* `laoban admin newtemplate` will turn the current package into a template (options can be used to change the name etc)
* Go to the created template directory and delete the files you don't want
* `laoban admin makeintotemplate` executed in the created template directory will update the `.template.json`

# Rarely used tasks

| Command                   | Purpose                                                                                                       |
|---------------------------|---------------------------------------------------------------------------------------------------------------|
| `laoban admin clearcache` | Used to clear the cache of the files loaded from urls. Unless they have changed there is little value in this | 
| `laoban admin profile`    | Show the time taken to do important tasks across all packages                                                 |





