# Adding `loaban` to an existing code base

The purpose of this little training exercise is to go through the process of adding `laoban` to an existing project

## Source

All the source code is [here](https://github.com/laoban-training/convertexistingproject).

If you clone it with `git clone git@github.com:laoban-training/convertexistingproject.git` you will get the 'starting
state'

## Exploring the project

The project is a simple typescript monorepo project. It has a `main` package that uses three libraries `lib1`,`lib2`
and `lib3`. These packages are located under 'modules' in the root of the project.

```
     lib1      lib3
       \       /
        lib2  /
          \  /
           main    
```   

Each library for this very simple example just export a string that is displayed by `main`

## Setting up with yarn

Yarn is set up to use `workspaces`. [For more info read here](https://yarnpkg.com/features/workspaces). In my view it is
very hard to work with a javascript/typescript monorepo without workspaces.

```shell
cd <to the project root directory>
yarn
```

## Compiling the project without `laoban`

This can be done with

```shell
cd modules/lib2
tsc --noEmit false --outDir dist
cd ../lib3
tsc --noEmit false --outDir dist
cd ../main
tsc --noEmit false --outDir dist
```

If we aren't using a tool like `laoban` we need to write scripts for even the simplest of activities. For example when
we compile we need to compile all the packages in the right order. This is done in the `compile` script in the
root `package.json` file. The right order for compilation is that `lib2` and `lib3` can be compiled in parallel
but `main` needs to be compiled after `lib2` and `lib3` are compiled.

Note that the scripts PROBABLY DON'T WORK ON YOUR MACHINE. Do you use Bash or Zsh or Windows CMD or Powershell or
something else provided by your IDE? It's also the case that usually in projects I have seen that scripts used in the
CI/CD pipeline are different to the scripts that run on the developer's machine.

Note also that we need another script for everything we want to do: test, compile, coverage analysis, integration tests,
performance testing...everything. And each time the project structure changes we need to maintain these scripts.

## Compiling the project with `laoban`

If we had `laoban` installed we could use `laoban compile`. Laoban will do the parallelism and the correct order. We
can't do that until we have installed `laoban` and configured the project.

## Setting up the project with `laoban`

If we are going to use the 'default templates' then this is very easy. The default templates are for a very precise
version of typescript (4.0.5). It is very easy to make a template for a different version of typescript and we explore
that below

```shell
npm i -g laoban
```

and check it works with

```shell
laoban --version
```

If you have problems read [Installing laoban](../laoban/INSTALLING.LAOBAN.md)

### Check for impact

When the default templates are used there may be a change to package dependencies. It is important to understand what
will happen when we do this

```shell
laoban admin analyze --showimpact
```

This produces the following

```json
{
  "C:\\git\\laobantraining\\convertexistingproject/modules/lib1": {},
  "C:\\git\\laobantraining\\convertexistingproject/modules/lib2": {
    "devDependencies": {"typescript": "^4.9.4 => ^4.0.5"}
  },
  "C:\\git\\laobantraining\\convertexistingproject/modules/lib3": {
    "devDependencies": {"typescript": "^4.9.3 => ^4.0.5"}
  },
  "C:\\git\\laobantraining\\convertexistingproject/modules/main": {
    "devDependencies": {"typescript": "^4.9.4 => ^4.0.5"}
  }
}
```

This tells us that using the default templates will downgrade the version of typescript. Obviously we don't want to do
this! We'll see how to solve this below.

### Generate the laoban files

Laoban needs to have a `laoban.json` file in the root of the project, and a `project.details.json` file in each package.
We could add them manually (which is error prone) or we can use the `laoban admin init` command to do it for us. If we
want to we can see 'what it would do' with the `--dryrun` option

```shell

```shell
laoban admin init --dryrun
```

In your IDE look at what this has done. There are now some new files. These don't work: they are 'test' files and only
exist for you to look at them and 'be happy' before actually doing the work

* .version.test.txt - contains the version of project. Currently 1.2.3
* .package.details.test.json - there is one for each [package](../laoban/PACKAGES.md)
* .laoban.test.json - the [laoban configuration file](../laoban/LAOBAN.JSON.md)

We can now run the command without the `--dryrun` option

```shell
laoban admin init
```

And we can get rid of the 'test' files now that laoban is configured using

```shell
laoban clean
```

# Playing with the project and laoban

You can explore what this has done in an IDE. Observe that it hasn't changed any of the `package.json` files yet. Also
remember that we haven't fixed the issue with typescript yet

You can see what packages exist, what template they are using and how they depend on each other with

```shell
laoban packages
```

You can compile them

```shell
laoban compile
```

You can test them with

```shell
laoban test
```

### Fix the typescript version

We want to make a new template that is like the old template, but with a different typescript version. For this we need
to select a directory that has `package.json` already set up the way we want. In this example we select `modules/main`.
We can then run

```shell
laoban admin updatetemplate --directory modules/main
```

Go look at the files system: you will see that a new directory has been created under `laoban/templates/typescript`. In
it there is a `.template.json` (a template definition) and a `package.json` (the template itself). These can be edited
by hand, although it is rare to modify the `.template.json` file manually.

Note how small the `package.json` in the template is: it just holds the 'differences' between it and the parent:

```json
{
  "devDependencies": {
    "typescript": "^4.9.4"
  }
}
```

### Check for impact again

```shell
laoban admin analyze --showimpact
```

This still shows some impact, but this is acceptable. In the past we had forgotten to update the typescript version
in `lib3`. This kind of error is common without tools like `laoban`

```json
{
  "C:\\git\\convertexistingproject/modules/lib1": {},
  "C:\\git\\convertexistingproject/modules/lib2": {},
  "C:\\git\\convertexistingproject/modules/lib3": {
    "devDependencies": {
      "typescript": "^4.9.3 => ^4.9.4"
    }
  },
  "C:\\git\\convertexistingproject/modules/main": {}
}

```

### `laoban update`

When we make changes to `package.details.json` we need to run `laoban update` to update the `package.json` files. What
this command does is
'make sure our project is in line with the template'. `package.json` is no longer the source of truth, it is a generated
file from `package.details.json` and the selected template in it

```shell
laoban update
```

After doing this it is a good idea to look with git and see if any 'damage' has been done. Check the `package.json`
files especially

### Check everything still works

```shell
yarn                        # should report 'no change'
laoban compile
laoban status              
```

The last command will display

```
                                                                compile
C:\git\laobantraining\convertexistingproject/modules/lib2       true
C:\git\laobantraining\convertexistingproject/modules/lib3       true
C:\git\laobantraining\convertexistingproject/modules/main       true
```

So far so good! Let's continue

```shell
laoban test
laoban status
```

But wait! The `laoban status` command shows that `lib1` had a problem with its tests

```
                                                                compile test
C:\git\laobantraining\convertexistingproject/modules/lib1               false
C:\git\laobantraining\convertexistingproject/modules/lib2       true    true
C:\git\laobantraining\convertexistingproject/modules/lib3       true    true
C:\git\laobantraining\convertexistingproject/modules/main       true    true
exit code 1
```

How can we find out what is the matter in `lib1`? We can scroll in the console and look, but there is a better way. We
can look at the `.session` directory. This is where `laoban` stores the output of the commands it runs.

In the file for `lib1` we see

```
$ jest --config jest.config.json
No tests found, exiting with code 1
Run with `--passWithNoTests` to exit with code 0
```

Basically `lib1` has been written without any tests. We can fix this by adding a test to `lib1` or by telling laoban to
not run tests on `lib1`. We'll do the latter. We can do this by editing the `package.details.json` file for `lib1` and
changing the `test` property to `false`. We can then run `laoban test` again. This time we see

```
                                                                compile test
C:\git\laobantraining\convertexistingproject/modules/lib1               false
C:\git\laobantraining\convertexistingproject/modules/lib2       true    true
C:\git\laobantraining\convertexistingproject/modules/lib3       true    true
C:\git\laobantraining\convertexistingproject/modules/main       true    true
exit code 1
```

What! What's going on? We've told laoban not to run tests on `lib1` but it still says that `lib1` has failed tests. The
answer is that the status shows `the last time tests were run` and we no longer run tests in `lib1`. We can fix this by
running `laoban clean` and then `laoban test` again. This time we see

```
                                                                compile test
C:\git\laobantraining\convertexistingproject/modules/lib1               true
C:\git\laobantraining\convertexistingproject/modules/lib2       true    true
C:\git\laobantraining\convertexistingproject/modules/lib3       true    true
C:\git\laobantraining\convertexistingproject/modules/main       true    true
```

As you can see the `laoban status` command gives us a nice view of the 'state' of our project. It can be used in the
CI/CD after a build to check that everything is ok. (it returns an exit code of 1 if there was a problem)

### Summary

The actual things we have done are

```shell
git clone https://github.com/laoban-training/convertexistingproject.git
cd convertexistingproject
yarn
laoban admin init
laoban admin analyze --showimpact                            # showed us that we had a problem with the typescript version
laoban admin updatetemplate --directory modules/main         # fixed the typescript version
laoban update                                                # updated the package.json files, and any other files required by the template
laoban compile
laoban test
laoban status                                                # showed us that we had a problem with the tests in lib1
# edit package.details.json for lib1 to set test to false
laoban clean
laoban test
laoban status
```

The only 'decision points' were 'what do we do about the downgraded typescript version' and 'how do we fix the tests
in `lib1`'

# Finished

The project is now configured to use `laoban`. You can now use `laoban` to build, test and publish your project.