# Store Block course template

Here you'll learn how to create awesome Store Framework blocks!

### TIP

To continue the store-block course lessons, you need to close its issue.

# 0. Before Starting

## 1. Presets

Make sure you have an IDE installed as well as Git.
Also, check if you have Node.js installed (and Yarn if you're using Mac) as well as the VTEX toolbelt.
For installing the toolbelt run:

``` ts
npm i -g vtex
```

or, for Mac users:

``` ts
yarn global add vtex
```

Check if it was correctly installed by typing vtex-v (Windows) or vtex (Mac).

## 2. Log in

Then you must log in to your VTEX account.
You can do this running the following command on your the terminal of your preference:

``` ts
vtex login appliancetheme
```

_where appliancetheme is your VTEX account name._
You can also check your login status with the 'who am I' command:

``` ts
vtex whoami
```

## 3. Workspace

Use or create and use a workspace by using:

``` ts
vtex workspace use workspacename
```

_A workspace name must contain only letters and numbers._

## 4. Access

To access your workspace, use this link: https://{workspace}--{account}.myvtex.com.

___

# 1. Introduction

## manifest.json

**vendor**: name of VTEX account (responsible for the app development) - use VTEX as the vendor to use native apps.
**name**: name of the app (only lowercase chars and hyphens (-).
**version**: X.x.x - major (breaking changes), minor (backward compatible new features), and patch (backward compatible bugfixes), specs at [semver.org](https://semver.org/).
**dependencies**: list of dependencies for the correct functioning of the app.

Example:

``` json
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

___

# 2. App linking and usage

## About React.js and StoreBlock

In order to develop a store front block it is necessary to do it using React.js.
According to VTEX, the usage should be focused on Hook API instead of using a class definition to components building.
In VTEX IO, the front end programming default language is **TypeScript**.

##  App linking

You must declare the new app at the theme's manifest.json file (within dependencies property).
And also, you must add your apps block at the desired .jsonc file.

## App usage

It is necessary to have at least two terminals opened and running `vtex link` , one for the theme and another for the custom block.

## App unlinking

Use `vtex unlink` to unlink the theme. To unlink the app, use `vtex unlink vtex.countdown@0.0.1` with the declaring the specifics app's name and version.

## App config via admin panel

Access https://yourworkspace--appliancetheme.myvtex.com/admin/apps/appliancetheme.countdown@0.0.1/ to see your app in VTEX Admin panel.
__

# 3. Making a block customizable

## Hooks

Hooks are APIs that allow us to use React features within functional components.

``` ts
const [count, setCount] = useState(0)
```

## _props_ interface

Defines _props_ TS typing that the component will receive.

``` ts
interface CountdownProps {
  // exampleProp: string,
  targetDate: string
}
```

## Block's schema

It is necessary to export a `schema` to allow user customization through Site Editor (VTEX Admin interface), similar to:

``` tsx
Countdown.schema = {
  title: 'editor.countdown.title',
  description: 'editor.countdown.description',
  type: 'object',
  properties: {
    targetDate: {
      title: 'Final date',
      description: 'Final date used in the countdown',
      type: 'string',
      default: null
    }
  }
}
```
## Getting user defined value
```ts
const Countdown: StorefrontFunctionComponent<CountdownProps> = ({ targetDate }) => {
  return (
    <div>
      <h1>{ targetDate }</h1>
    </div>
  )
}
```

**TIP:** access the [Site Editor](yourworkspace--appliancetheme.myvtex.com/admin/cms/site-editor) (don't forget to change `yourworkspace--appliancetheme` by you own values).
