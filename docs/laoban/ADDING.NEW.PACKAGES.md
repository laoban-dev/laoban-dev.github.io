# Adding new packages

# Overview

Laoban will add the appropriate details to a directory if you call the following command:
```shell
laoban admin newpackage <directory>
```
You can call it
* from the directory you want to add (just `laoban admin newpackage`)
* from a parent directory (e.g. `laoban admin newpackage modules\mynewpackage`)

# What does it do?

This command will create a `package.details.json` file in the directory you specify. It will use information from
a `package.json` file if that exists to work out things like names/descriptions/links and the template to use

# Examples

```shell
laoban admin newpackage                        # creates a package.details.json in the current directory
laoban admin newpackage modules\mynewpackage   # creates a package.details.json in the modules\mynewpackage directory
laoban admin newpackage --template javascript  # creates a package.details.json in the current directory using the javascript template
```

