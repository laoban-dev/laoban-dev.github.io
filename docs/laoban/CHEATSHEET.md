# Installing

* `npm i -g laoban@latest` will install `laoban`

# Laoban Cheatsheet

| Command | Guarded? | Purpose |
| --- | --- | --- |
| `laoban update` | No |Used when the version number in `version.txt` has been changed to update the project, and is also used when a package.details.json file is changed 
| `laoban compile -asl` | details.compile | Compile all the typescript projects in the correct order 
| `laoban test -asl` | details.test | Test all the typescript projects in the correct order
| `laoban publish -asl` | details.publish | Publishes the projects to npm
| `laoban status -a` | No |  Show the status of important commands (compile/test) across all projects
| `laoban run 'rm -rf node_modules'` | No | In linux remove all the node modules from the project (be careful)

# laoban admin Cheatsheet
[aoban-admin](https://www.npmjs.com/package/@laoban/admin) is the recommended way to [getting started](GETTING.STARTED.md)
with existing projects

| Command |  Purpose |
| ---  --- |
| `laoban admin projects` | Lists the found projects in the monorepo (with package.json files) and what is the recommended template
| `laoban admin init --dryrun` |  Generates 'test' files that allow you to look at them before using them
| `laoban admin init --force` |  Sets up your project with `laoban.json` and `package.details.json` that should work
| `laoban admin newpackage <packageLocation> -p <packageName>` |  Creates a new package in the monorepo
| `laoban admin newtemplate`  |  creates a new template with a default name from the files in the current directory placing the template under the `templates` directory (under the current directory) 
| `laoban admin newtemplate --directory <directory>`  |  creates a new template with a default name from the files in the `directory` placing the template under the `templates` directory (under the current directory) 
| `laoban admin newtemplate -t <templatename> --directory <directory> --template <templatesdirectory> ` |  creates a new template called `<templatename>` from the files in the `<directory>`, placing the template under the `templatesdirectory` directory 
| `laoban admin makeintotemplate  --directory <directory> ` |  Turns the <directory> into a template 
| `laoban admin clearcache ` |  Used to clear the cache of the files loaded from urls. Unless they have changed there is little value in this 





