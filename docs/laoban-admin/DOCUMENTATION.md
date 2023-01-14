# Setting up an existing project

For more details [read here](Existing.md). Overview:

* Open the command line, and change directory to the root of the git repo
* Use `laoban admin packages` to see what packages (directories with package.json) are in the mono-repo
* Use `laoban admin init --dryrun` to see what laoban files will be generated
* Use `laoban admin init --force` To actually generate the laoban files
* Use `laoban update` to generate package.json
* Use `yarn` to make symbolic links
* Use `laoban compile` to compile the project

# Starting with a greenfield site

For more details [read here](Greenfield.md). Overview:
* Open the command line, and change directory to the root of the git repo
* Use `laoban admin init --dryrun` to see what laoban files will be generated
* Use `laoban admin init --force` To actually generate the laoban files
* Edit the `laoban.json` file: it will have errors in it as it cannot 'scrape' an existing project

# Creating a new package

* Open the command line, and change directory to somewhere 'above' the package you want to create
* Use `laoban admin newpackage <pathToDirectory>` to create a new package
* use `laoban admin newpackage --help` for details of options



# What next

## Using laoban to test/compile

See [https://www.npmjs.com/package/laoban](laoban) documentation

# Making your own 'inits'
At the moment it is still extremely experimental. It works for the default inits. It needs some work to make
it more suitable for general use. The [documentation is here](customInits.md)

