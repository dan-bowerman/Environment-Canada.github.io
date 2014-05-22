---
layout: index-en
title: EC - RAMP
---
{% include JB/setup %}

# Getting Started

If you are working behind a proxy, you may want to look at the [proxy troubleshooting guide] (http://wet-boew.github.io/wet-boew/docs/proxy-en.html).

## I want to use RAMP on my website
1. Grab the latest stable release from the [downloads page] (http://environment-canada.github.io/docs/versions/downloads-en.html).

## I want to customize RAMP and design a new RAMP template
1. [Install NodeJS] (http://nodejs.org/) for your platform.
2. Create a directory for your project.
3. Install the Grunt CLI, Yeoman, and Bower packages globally by running `npm install -g grunt-cli yo bower` from the command line console (Command Prompt on Windows, Terminal on OS X, or other shell environment on Linux or Unix).
4. Install the WET theme generator by running `npm install -g generator-wet-boew-theme`.
5. Run `yo wet-boew-theme` from the command prompt, and answer the questions at the prompt to setup your theme project.

## I want to report a bug or ask a question about WET
1. [Create a GitHub account] (https://help.github.com/articles/signing-up-for-a-new-github-account).
2. Ask a question relating to the style guide by creating a new issue on GitHub.
3. Report a bug, ask a question or request a feature relating to RAMP by filling out [a new issue] (https://github.com/Environment-Canada/RAMP-AF/issues/new).

## I want to fix or improve WET as a developer
1. [Install NodeJS] (http://nodejs.org/) for your platform.
2. [Create a GitHub account] (https://help.github.com/articles/signing-up-for-a-new-github-account).
3. Follow GitHub's guides on [setting up Git] (https://help.github.com/articles/set-up-git).
4. Fork the latest [RAMP repository] (https://github.com/Environment-Canada/RAMP-AF) by following the GitHub forking guide. Use the upstream URL of `https://github.com/wet-boew/wet-boew.git` in step 3.
5. Run `./script/setup` from the `wet-boew` directory in your command line console.
6. Run grunt to build the project. You may run `grunt --help` to see the build target descriptions.
7. The latest files will now be compiled to the `dist/` folder.
8. To contribute back, follow the instructions on how to create a pull request.