---
title: Bazel Python TUI Bootstrap
description: Packaging a Python Textual TUI application with Bazel across Linux, macOS and Windows.
date: 2024-09-01
tags:
  - bazel
  - python
  - textual
  - tui
layout: layouts/post.njk
---

I recently discovered Textual, a Python library for building terminal UI applications and thought it would be fun to figure out how to package an application that is built with textual for Linux, macOS and Windows. This is what I came up with.

--excerpt--

I decided that in addition to [Textual](https://github.com/Textualize/textual), I wanted support for for numpy (to prove that Python C extensions work), runfiles, so files that are shipped with the binary, like data files, support for Protobuf, ability to call into custom C/C++ code from Python and ability to launch shell scripts.

The results are made public in a git repository: [GitHub](https://github.com/sschaetz/bazel-textual-experiments).


![Red and Black Crane](red-and-black-crane.jpg)

Red and Black Crane by [Pixabay](https://www.pexels.com).

## Textual

At first, Textual struck me as slightly weird, why style it with something like CSS and why are all controls so big, there is not a lot of space in the terminal.

The power of the CSS implementation becomes clear once one updates the styles while the app is running and it changes automatically. Very powerful and optimizes the iteration time on the layout significantly.

The way this is implemented in the experimental repo is as follows. Run in 2 terminals, in the debug terminal we see the app events, in the app terminal we see the app itself and can interact with it:

```bash
# debug terminal:
bazel run //src:textual -- console

# app terminal:
bazel run //src:example --run_under "//src:example_dev -- run --dev"
```

Note that for the app terminal we run a special target that has the runfiles added to the textual dev tool.

The default control size can be kept in check with something like this:

```css
padding: 0;
margin: 0;
border: none;
```

Lots of cool apps are being built with it.

## Cross Platform

Cross platform is always an interesting one. How to run and test? Install VMs, dust off that old computer that runs a dangerously old outdated version of Windows? I took a bit of a shortcut and chose GitHub actions to run, test and package the my example code. It's kind of nuts that GitHub gives you access to [various operating systems](https://github.com/actions/runner-images) for free. We set up a matrix and use `bazel run requirements.update` to generate the lock file for each platform.

Alternatively one could use Docker to run the update command locally but running through GitHub is less effort.

## Bazel

Now for the fun part: what build-system to use? To be frank, that is not really a question for me in 2024. Bazel is my default, so it is an obvious choice. It has a shallow learning curve but I know the basics. And I am excited to try the relatively new [bzlmod](https://bazel.build/external/overview) to manage the various dependencies needed. It also is cross-platform compatible, and I'm using [bazelisk](https://github.com/bazelbuild/bazelisk) and the [corresponding](https://github.com/bazelbuild/setup-bazelisk) GitHub action step.

There are a number of Bazel rules to pull in:

* [rules_python](https://github.com/bazel-contrib/rules_python) not only defines rules for binaries and libraries but also pulls in cross-platform stand-alone Python executables from [indygreg](https://github.com/astral-sh/python-build-standalone) an invaluable project; This is also where some of the trickier stuff is going on, related to pip. Using [pip.parse](https://rules-python.readthedocs.io/en/latest/api/rules_python/python/extensions/pip.html) we teach bazel that depending on the OS there are different lock files it needs to maintain.
* [rules_proto](https://github.com/bazelbuild/rules_proto) adds the protobuf compiler.
* [rules_pkg](https://github.com/bazelbuild/rules_pkg) allows us to create packages (tar, zip), we use it to bundle the python script and all of its dependencies including the Python interpreter in a movable package
* [pybind11_bazel](https://github.com/pybind/pybind11_bazel) adds abilityt to call C/C++ function from Python

As a bonus I added calling a shell script. This way the textual app could become a hub for calling into various existing code and showing the status or the output.

## Packaging

Packaging I kept pretty straight forward for now, we simply use `rules_pkg` to tar everything up. We test the package by running it on a fresh GitHub action runner to make sure we are not relying on any files outside the package.

## Issues

Windows has this awkward maximum path length limitation, so long paths should be [avoided](https://bazel.build/configure/windows). I work around that by adding `startup --output_user_root=C:/tmp` to `.bazelrc` in CI.

The pip requirements lock file functionality does not work flawlessly for Windows either, it seems to generate incorrect comments which cause `bazel test requirements_test` if they are not cleaned up. Also updating these requirements is a manual process where we have to add a requirement to `requirements.in`, push the change and get the updated lock files generated by GitHub. This could be automated in some way.
