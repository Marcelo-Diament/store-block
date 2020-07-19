# Store Block course template

Here you'll learn how to create awesome Store Framework blocks!

### TIP

To continue the store-block course lessons, you need to close its issue.

## 0. Before Starting

### 1. Presets

Make sure you have an IDE installed as well as Git.
Also, check if you have Node.js installed (and Yarn if you're using Mac) as well as the VTEX toolbelt.
For installing the toolbelt run:

``` sh
npm i -g vtex
```

or, for Mac users:

``` sh
yarn global add vtex
```

Check if it was correctly installed by typing vtex-v (Windows) or vtex (Mac).

### 2. Log in

Then you must log in to your VTEX account.
You can do this running the following command on your the terminal of your preference:

``` sh
vtex login appliancetheme
```

_where appliancetheme is your VTEX account name._
You can also check your login status with the 'who am I' command:

``` sh
vtex whoami
```

### 3. Workspace

Use or create and use a workspace by using:

``` sh
vtex workspace use workspacename
```

_A workspace name must contain only letters and numbers._

### 4. Access

To access your workspace, use this link: https://{workspace}--{account}.myvtex.com.

___

## 1. Introduction

### manifest.json

* **vendor**: name of VTEX account (responsible for the app development) - use VTEX as the vendor to use native apps.
* **name**: name of the app (only lowercase chars and hyphens (-).
* **version**: X.x.x - major (breaking changes), minor (backward compatible new features), and patch (backward compatible bugfixes), specs at [semver.org](https://semver.org/).
* **dependencies**: list of dependencies for the correct functioning of the app.

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

## 2. App linking and usage

### About React.js and StoreBlock

In order to develop a store front block it is necessary to do it using React.js.
According to VTEX, the usage should be focused on Hook API instead of using a class definition to components building.
In VTEX IO, the front end programming default language is **TypeScript**.

###  App linking

You must declare the new app at the theme's manifest.json file (within dependencies property).
And also, you must add your apps block at the desired .jsonc file.

### App usage

It is necessary to have at least two terminals opened and running `vtex link` , one for the theme and another for the custom block.

### App unlinking

Use `vtex unlink` to unlink the theme. To unlink the app, use `vtex unlink vtex.countdown@0.0.1` with the declaring the specifics app's name and version.

### App config via admin panel

Access https://yourworkspace--appliancetheme.myvtex.com/admin/apps/appliancetheme.countdown@0.0.1/ to see your app in VTEX Admin panel.
___

## 3. Making a block customizable

### Hooks

Hooks are APIs that allow us to use React features within functional components.

``` tsx
const [count, setCount] = useState(0)
```

### _props_ interface

Defines _props_ TS typing that the component will receive.

``` tsx
interface CountdownProps {
  // exampleProp: string,
  targetDate: string
}
```

### Block's schema

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

### Getting user defined value

``` tsx
const Countdown: StorefrontFunctionComponent<CountdownProps> = ({ targetDate }) => {
  return (
    <div>
      <h1>{ targetDate }</h1>
    </div>
  )
}
```

**TIP:** access the [Site Editor](yourworkspace--appliancetheme.myvtex.com/admin/cms/site-editor) (don't forget to change `yourworkspace--appliancetheme` by you own values).

___

## 4. Countdown feature

### the useState React's hook

First of all, it is necessary to import this hook, by declaring:

``` tsx
import React, { useState } from 'react'
```

It is used to update or consume the component's **state** (which represents its current state).
This hook returns a pair - the current state and a function used to update it.

``` tsx
const [count, setCount] = useState(0)
```

_where count retains the current value, setCount is the method to update its state, and 0 is the initial value._

### Countdown feature - step by step

So, blending all the steps until now, we have:

``` tsx
// Imports
import React, { useState } from 'react'
import { TimeSplit } from './typings/global'
import { tick } from './utils/time'

// CountdownProps interface whith targetDate prop with string type definition
interface CountdownProps {
  targetDate: string
}

// Default target date
const DEFAULT_TARGET_DATE = (new Date('2020-12-31')).toISOString()
// The Countdown (with the optional targetDate param and the time remaining update function)
const Countdown: StorefrontFunctionComponent<CountdownProps> = ({ targetDate = DEFAULT_TARGET_DATE }) => {
  const [timeRemaining, setTime] = useState<TimeSplit>({
    hours: '00', 
    minutes: '00', 
    seconds: '00'
  })

  tick(targetDate, setTime)
  
  return (
    <div>
      <h1>{ `${timeRemaining.hours}:${timeRemaining.minutes}:${timeRemaining.seconds}` }</h1>
    </div>
  )
}

// Countdown Schema (that allows user to define its value)
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

export default Countdown

```

___

## 5. Countdown configurable style

### CSS Handles

Allows to use CSS class to customize the store's components (through `vtex.css-handles` app).
It is possible to create flexible layouts by using CSS handles names on elements HTML.

To import CSS Handles just include this code snippet on your component .tsx file:

``` tsx
import { useCssHandles } from "vtex.css-handles"
```

With useCSSHandles imported, we must define the CSS Handles:

``` tsx
const CSS_HANDLES = ["countdown"]
```

_In this case we'll just declare the countdown handle._

So we use the useCSSHandle component to define the countdown handle:

``` tsx
const handles = useCssHandles(CSS_HANDLES)
```

Finally, we must set the className as an attribute of the HTML tag:

``` tsx
<div className={ `${handles.countdown}` }>
  Content
</div>
```

### Tachyons

**Tachyons** is a functional CSS framework, which does not have 'pre-built' UI components.
Its main concept is to 'explode' our style in minor and reusable parts ([Subatomic Design System](https://daneden.me/blog/2018/subatomic-design-systems/)).
It is possible to change Tachyons' definitions by adding a JSON file in the `styles/config` folder.

___

## 6. Internationalization practices in VTEX IO

### Introduction

Blocks must follow the good localization practices and **must not display hardcoded strings**.
But not all the text (for each language) must be translated by you. Let's learn about app internationalization.

### Messages

Messages hold all the translation services responsibilities. There is a flow in which each text translation will be checked on user-defined context, then in-app translations, and, finally, through the automatic translation system.

Its configuration files are placed in the `./messages` folder. Within this folder, there are three main files: `pt.json` , `en.json` , and `es.json` . To avoid ambiguities, there is the `context.json` file - it is a JSON file where the message is the key and its translation is the value.

The Messages feature requires the [react-intl lib](https://github.com/formatjs/formatjs) (it is suggested to check the libs documentation out). Add the lib by running this command on the terminal:

``` sh
yarn add react-intl
```

In order to use the FormattedMessage component, we must import it:

``` tsx
import { FormattedMessage } from 'react-intl'
```

Then we add a new prop in our interface:

``` tsx
interface CountdownProps {
  title: string,
  targetDate: string
}
```

We must also define our const with its value:

``` tsx
const titleText = title || <FormattedMessage id="countdown.title" />
```

At last, we must render it. So we'll define it as a prop and include an outer container:

``` tsx
const Countdown: StorefrontFunctionComponent<CountdownProps> = ({
  title,
  targetDate,
}) => {
  const [timeRemaining, setTime] = useState<TimeSplit>({
    hours: '00',
    minutes: '00',
    seconds: '00',
  })

  const titleText = title || <FormattedMessage id="countdown.title" />
  const handles = useCssHandles(CSS_HANDLES)

  tick(targetDate, setTime)

  return (
    <div className={ `${handles.container} t-heading-2 fw3 w-100 c-muted-1` }>
      <div className={ `${handles.title} db tc` }>{titleText}</div>
      <div className={ `${handles.countdown} db tc` }>
        { `${timeRemaining.hours}:${timeRemaining.minutes}:${timeRemaining.seconds}` }
      </div>
    </div>
  )
}
```

Observe that when we include new CSS Handles we must declare them in the CSS_HANDLES array.
Also, once we're using a new prop, we must added it to the app schema.

Example:

``` tsx
Countdown.schema = {
  title: 'editor.countdown.title',
  description: 'editor.countdown.description',
  type: 'object',
  properties: {
   title: {
    title: 'I am a title',
    type: 'string',
    default: null,
  },
    targetDate: {
      title: 'Final date',
      description: 'Final date used in the countdown',
      type: 'string',
      default: null,
    },
  },
}
```
Now we can check translation in action by adding the following query parameter: `/?cultureInfo=es-ar` or `/?cultureInfo=pt-br`.

___

