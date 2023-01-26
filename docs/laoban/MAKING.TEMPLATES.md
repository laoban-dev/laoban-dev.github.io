# Making templates

[Templates](TEMPLATES.md) are defined by a [template control file](TEMPLATES.md#templateControlFile).
This file is a JSON file that defines the files that should be copied into the package directory. 
It also defines how the files should be processed during that copying.

## Making a new template that is a modification of an existing template

A very common use case is that you want to use a template that is very similar to an existing template, but you want to 
make a few changes. The most common is to change a version of a dependency, or to add new dependencies.

To do this, you can use the `laoban admin updatetemplate` command. This will create a new template that is a copy of an existing template.

