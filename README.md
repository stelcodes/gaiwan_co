# Gaiwan GmbH

This repository consists the code for our website https://gaiwan.co

We are a provider of technological expertise grown out of the consulting and development work of Arne Brasseur.

## Tech stack

We love Clojure, and this site is built with it! 

We're using [Babashka](https://github.com/borkdude/babashka) and [Bootleg](https://github.com/retrogradeorbit/bootleg) to build a static site.

##  Architechture

All the source files are located in `src/gaiwan` directory and static resources in the `resources` directory.

The entry point for the generation of the site is `build.clj`

The site gets generated inside a `_site` directory. This directory is added to gitingore because we don't want to mix the build and source history. Therefore we commit the `_site` directory to a `gh-pages` branch using a nifty feature of git called as worktrees.

Our Github repo has been configured to deploy the site directly from the `gh-pages` branch. Therefore by commiting and pushing changes to this branch would cause Github to deploy the new version of the site.

## Local development

First install bootleg using

```
sudo bash bin/install_bootleg.sh
```

Then clone the repository and run:

```bash
sudo apt install inotify-tools
make watch 
# this will generate a `_site` folder
# and watch the `src` and `resources` directory
# and rebuild the site on any modifications
```

If you don't have `bb` installed, our wrapper script will automatically install babashka in the local project directory.

Open up another terminal window and run

```bash
# cd to project directory
make serve
```

This will run a local server serving the `_site` directory on http://localhost:8001

The `resources` directory is copied to the final site as is.

## Deployment

For first time deployment there is an extra step involved where you need to run the following commands:

```bash
rm -rf _site
git worktree add -B gh-pages public origin/gh-pages
```

Once you're satisfied with the changes, deploy the site live by running:

```
make deploy
```

## Enhancements

Currently the setup is extremely simple and minimal. Everytime you make a change you need to rerun the build command. This is tiresome and we need to find a way to autobuild on file changes.

An interesting solution would be to move the entire build step into babashka's build file and use the babashka-file-watcher pod.

Another problem is that the css/js assets are not being hashed. This can lead to caching issues. We can use babashka and `sha1sum` utility present on linux systems to build on this.

## License

TODO
