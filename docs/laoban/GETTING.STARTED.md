# Getting started

It is possible to do all the following tasks by creating files yourself. In essence all you need to do is 
copy a `laoban.json` from another project, and for each package add a `package.details.json`. However this is slow and 
error prone

`laoban admin` provides a number of commands to help you get started.

## Install

```shell
npm i -g laoban
```

[If you have issues read](INSTALLING.LAOBAN.md) 

## Read the training course

If this is your first time, it helps to practice on a training project. The training project is a simple project with a
monorepo structure. It is a good starting point for learning how to use `laoban`.

[Details are here](../training/EXISTING.md)

## Start with a clean system

Make a new git branch for this, and make sure that everything is commited: you want to be able to see what 
has happened and be able to roll back!

## Create `laoban.json` and `package.details.json` files

```shell
laoban admin init
```
Take a look at what has changed. None of the `package.json` files have been changed. You can look at the
created files. 

It is quite likely that you have the 


## Check for impact

```shell
laoban admin analyze --showimpact
``` 
This will show you if running `laoban update` would change your package.json. Probably it will! Unless you
use precisely the same version of typescript as laoban... The changes are typically
the version of typescript/react, or parts of the package.json that the default templates don't know about.

## Update the template

Pick one of the packages and use it to update the template. 

```shell
laoban admin updatetemplate --directory <the directory of the package you selected>
```
If you want to edit the template, you can do that locally now (it's under the `templates` directory). Find the `package.json`
and add any bits you want.

## Check for impact again

```shell
laoban admin analyze --showimpact
``` 

As long as you have 'one version of typescript/react/etc' you should quickly get to the point where the impact is either
'nothing' or something you don't care about

## Update the packages

```shell
laoban update
```

## Check everything compiles/tests

I would typically do the following here
```shell
yarn
laoban update
laoban compile
laoban test
laoban status
```

