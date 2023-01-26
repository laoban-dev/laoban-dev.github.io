# Making new templates

WARNING THIS IS UNDER DEVELOPMENT AND NOT YET IMPLEMENTED

## Making a new template that is a modification of an existing template

### Updating a package that is a  user of the template

The simplest way is to pick a package that used the template

* Update the package: making the files look like you want them to.
    * For example updating a dependency version
* run the `laoban admin maketemplate <dir>` command either 'in' the directory or specifying it as an argument

This either creates or updates the template locally (it creates if a local copy doesn't exist). This adds the minimum
information that it
can using as much information from the template control file as possible.

If you want to make this a new template (for example the original was `typescript` and you want this to
be `typescript_4.1.7`) you can use the `--template` argument to specify the name of the new template.

```shell
laoban admin newtemplate --originaltemplate <name> --template <name> --source 
```

Updates the template using the information in the package.details.json file.

| Directory type             | --originaltemplate            | --template                    | --source                 | 
|----------------------------|-------------------------------|-------------------------------|--------------------------|
| has `.template.json`       | <directory>       (error)     | <directory>                   | <directory>              |
| has `package.details.json` | package.details.json#template | package.details.json#template | <directory>      (error) |
| parent of `.template.json` | each <directory> (error)      | each <directory> (error)      | each <directory> (error) |
| other (error)              | (error)                       | (error)                       | (error)                  |

# NOTE

We really want parent templates now. relatively easy to do and will help a lot with maintenance. The .template.json is
smaller and simpler

But that means we also need to be able to see what the effective template.json file is

# Steps:

* Add the # logic. 
* Make it so that we can reference the package.details.json in the jsonMergeInto
* Add the ability to be a parent of a template and thus simplify existing templates
* Add the ability to see the effective template.json file
* Make tests for these and make them pass

# For consideration

## The '#' character in jsonMergeInto()

In the json merge is we could have a '#' character that means 'merge this part of the file with the parent object'. We
could dramatically simplify the logic (removing the need for complex object variables and the comma logic) and make it
less about 'package.json'. It would extend into 'pom.xml' as well with the same mechanism.

## Name and value

When we say 'typescript' by default we mean 'the latest version of the template'. But we also want to be able to support
older versions. Do we want to use CAS for this? That removes many hacking vectors but reduces readability

The obvious answer is that [designing for security](DESIGN.FOR.SECURITY.md) is important!!



