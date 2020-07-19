# Store Block course template

Here you'll learn how to create awesome Store Framework blocks!

# 0. Initial Notes

## Before Starting

### 1. Presets
Make sure you have an IDE installed as well as Git.
Also check if you have Node.js installed (and Yarn if you're using Mac) as well as the VTEX toolbelt.
For installing the toolbelf run:
```
npm i -g vtex
```
or, for Mac users:
```
yarn global add vtex
```
Check if it was correctly installed by typing vtex-v (Windows) or vtex (Mac).

### 2. Log in
Then you must log in to your VTEX account.
You can do this running the following command on your the terminal of your preference:
```
vtex login appliancetheme
```
_where appliancetheme is your VTEX account name._
You can also check your login status with the 'who am I' command:
```
vtex whoami
```

### 3. Workspace
Use or create and use a workspace by using:
```
vtex workspace use workspacename
```
_A workspace name must contain only letters and numbers._

### 4. Access
To access your workspace, use this link: https://{workspace}--{account}.myvtex.com.

## 1. Introduction

### manifest.json

**vendor**: name of VTEX account (responsible for the app development) - use VTEX as the vendor to use native apps.
**name**: name of the app (only lowercase chars and hyphens (-).
**version**: X.x.x - major (breaking changes), minor (backward compatible new features), and patch (backward compatible bugfixes), specs at [semver.org](https://semver.org/).
**dependencies**: list of dependencies for the correct functioning of the app.

Example:
```
{
  "vendor": "vtex",
  "name": "countdown",
  "version": "0.0.1",
  "title": "Countdown",
  "description": "Countdown component",
  "defaultLocale": "pt-BR",
  "builders": {
    "messages": "1.x",
    "store": "0.x",
    "react": "3.x"
  },
  "mustUpdateAt": "2019-04-02",
  "scripts": {
    "postreleasy": "vtex publish --verbose"
  },
  "dependencies": {
    "vtex.styleguide": "9.x",
    "vtex.css-handles": "0.x"
  },
  "$schema": "https://raw.githubusercontent.com/vtex/node-vtex-a pi/master/gen/manifest.schema"
}
```