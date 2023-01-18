# Adding `loaban` to an existing code base

## Source

All the source code is [here](https://github.com/laoban-training/convertexistingproject). 

If you clone it with `git clone git@github.com:laoban-training/convertexistingproject.git` you will get the 'starting state'

## Exploring the project

The project is a simple typescript monorepo project. It has a `main` package that uses two libraries `lib1` and `lib2`.
These packages are located under 'modules' in the root of the project.

## Setting up with yarn

Yarn is set up to use `workspaces`. [For more info read here](https://yarnpkg.com/features/workspaces). In my
view it is very hard to work with a javascript/typescript monorepo without workspaces.

At the root of the directory in the command line type `yarn` to install all the dependencies.

## Compiling the project without `laoban`

This is a little challenging. Try it

* cd to modules/lib1
* run `tsc --noEmit false --outDir dist`
* cd to modules/lib2
* run `tsc --noEmit false --outDir dist`
* cd to modules/main
* run `tsc --noEmit false --outDir dist`

Note that there was no parallelism (it would be possible to compile lib1 and lib2 in parallel). Also note that we would
need to write a script to execute these in the correct order. If the structure of the project changes, we would need to
maintain the script.

Note also that we need another script to test the code. Another to publish the code. And so on... And each time the project 
structure changes we need to maintain these scripts.

## Compiling the project with `laoban`

This would just be `laoban compile`. Laoban will do the parallelism and the correct order.


## Setting up the project with `laoban`

If we are going to use the 'default templates' then this is very easy. The default templates are for a very 
precise version of typescript (4.0.5). It is very easy to make a template for a different version of typescript 
and we explore that below

* open a command shell
* Install laoban with `npm i -g laoban`
* cd to the root of the project


### Check for impact

When the default templates are used there may be a change to package dependencies. It is important to understand what will
happen when we do this
```shell
laoban admin analyzepackages --showimpact
```
This produces the following
```json
{
  "C:\\git\\laobantraining\\convertexistingproject/modules/lib1": {
    "devDependencies": {"typescript": "^4.9.4 => ^4.0.5"}
  },
  "C:\\git\\laobantraining\\convertexistingproject/modules/lib2": {
    "devDependencies": {"typescript": "^4.9.4 => ^4.0.5"}
  },
  "C:\\git\\laobantraining\\convertexistingproject/modules/main": {
    "devDependencies": {"typescript": "^4.9.4 => ^4.0.5"}
  }
}
```

This tells us that using the default templates will downgrade the version of typescript. For now we will accept this,
although we will see how to solve this later.

### Generate the laoban files


* `laoban admin init --force` - this will create a `laoban.details.json` file in the root of the project, and a `project.details.json` in each of the three packages

You can explore what this has done in an IDE. Observe that it hasn't change any of the `package.json` files yet. You can also see what has been created by

* `laoban packages` - this will show you the packages in the project, their templates, and their dependencies

### Fix the typescript version

We want to make a new template that is like the old template, but with a different typescript version. For this we need
to select a directory that has `package.json` already set up the way we want. We can then run

* ``


### `laoban update`

When we make changes to `package.details.json` we need to run `laoban update` to update the `package.json` files.

After doing this it is a good idea to look with git and see if any 'damage' has been done.

### `yarn`

I usually run `yarn` after `laoban update` to make sure that the symbolic links are correct. This just avoids any surprises later.

### `laoban compile`

This will compile the project. It will do the parallelism and the correct order. 

### `laoban test`

This will test the project. It will do the parallelism and the correct order. 

### `laoban status`

This will show you the results of the compile and the test.

```shell
                                                                compile test
C:\git\laobantraining\convertexistingproject/modules/lib1       true    true
C:\git\laobantraining\convertexistingproject/modules/lib2       true    true
C:\git\laobantraining\convertexistingproject/modules/main       true    true
```

## Finished

This simple case had the same version of typescript as the default template. We have completed the set up.


# Making a template for a different version of typescript

Clearly it's unlikely that we have the correct version of typescript, and in fact for any template that we use
from the default templates (typescript_react for example) it's probable that we will want to use different versions.

## Making a template

