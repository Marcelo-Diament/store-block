# Store Block Course

A step by step guide to understanding Store Block by implementing a Countdown block.

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

Check if it was correctly installed by typing `vtex-v` (Windows) or `vtex` (Mac).

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

In order to develop a store front block, it is necessary to use React.js.
According to VTEX, the usage should be focused on Hook API instead of using a class definition to components building.
In VTEX IO, the front end programming default language is **TypeScript**.

###  App linking

You must declare the new app at the theme's `manifest.json` file (within dependencies property).
And also, you must add your apps block at the desired `.jsonc` file.

### App usage

It is necessary to have at least two terminals opened and running `vtex link`, one for the theme and another for the custom block.

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

**TIP:** access the [Site Editor](yourworkspace--appliancetheme.myvtex.com/admin/cms/site-editor) to define values (don't forget to change `yourworkspace--appliancetheme` by you own values).

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

So, combining all the steps until now, we have:

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

_In this case, we'll just declare the countdown handle._

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
Its main concept is to 'explode' the style in minor and reusable parts ([Subatomic Design System](https://daneden.me/blog/2018/subatomic-design-systems/)).
It is possible to change Tachyons' definitions by adding a JSON file in the `styles/config` folder.

___

## 6. Internationalization practices in VTEX IO

### Introduction

Blocks must follow the good localization practices and **must not display hardcoded strings**.
But not all the text (for each language) must be translated by you. Let's learn about app internationalization.

### Messages

Messages hold all the translation services responsibilities. There is a flow in which each text translation will be checked on user-defined context, then in-app translations, and, finally, through the automatic translation system.

Its configuration files are placed in the `./messages` folder. Within this folder, there are three main files: `pt.json`, `en.json`, and `es.json`. To avoid ambiguities, there is the `context.json` file - it is a JSON file where the message is the key and its translation is the value.

The Messages feature requires the [react-intl lib](https://github.com/formatjs/formatjs) (it is suggested to check the documentation of the libs out). Add the lib by running this command on the terminal:

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
Also, once we're using a new prop, we must add it to the app schema.

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

## 7. Componentizing the countdown block

### Componentization

Since there are two main elements in our component (the countdown itself and its title), in order to achieve a more flexible layout, we can split it into two components - the countdown and the title. So our new component (title), needs its own interface.

### Interface

As in OOP, an interface defines a contract, with well-defined restrictions on how the blocks will work together. Using interfaces enhances our customization power.

To define an app in the interface, the component property is responsible to define the React component to be used. So it is important that the component name be the same as the file name inside the `react/` folder. Take a look at this example ( `interfaces.json` ):

``` tsx
{
  "countdown": {
    "component": "Countdown"
  }
}
```

### Adding a new component

The process is the same we just did with Countdown component. But, here is a resume of the process:

1. **Create the component file** (the first letter must be uppercase) in the `react/` folder.
2. Import React from 'react' and other **necessary imports**.
3. Define the **component interface**.
4. Define **CSS handles** const.
5. Define the **component function**
6. **Export default Component**

So, for our Title component, we've got the following code ( `react/Title.tsx` ):

``` tsx
import React from "react"
import { FormattedMessage } from "react-intl"
import { useCssHandles } from "vtex.css-handles"

interface TitleProps {
    title: string
}

const CSS_HANDLES = ["title"] as const

const Title: StorefrontFunctionComponent<TitleProps> = ({ title }) => {
    const handles = useCssHandles(CSS_HANDLES)
    const titleText = title || <FormattedMessage id="countdown.title" />

    return (
        <div className={ `${handles.title} t-heading-2 fw3 w-100 c-muted-1 db tc` }>
            {titleText}
        </div>
    )
}

Title.schema = {
    title: "editor.countdown-title.title",
    description: "editor.countdown-title.description",
    type: "object",
    properties: {
        title: {
            title: "I am a title",
            type: "string",
            default: null,
        },
    },
}

export default Title
```

### Updating interfaces.json

All you have to do is to add your component to the JSON file ( `./store/interfaces.json` ).

### Component internationalization

Update each language file ( `./messages/en.json` , `./messages/es.json` , and `./messages/pt.json` ).

### Adding the new block to store home

Just add the component ( `countdown.title` in our lesson) in the block's property of `./store-theme/store/blocks/home/home.jsonc` .

___

## 8. Connecting back end and front end

### Introduction

VTEX IO uses [GraphQL](https://graphql.org/) to data transfer processes. To make the GraphQL API integration easier, we'll use **Apollo Client**, a state management lib that offers native React integration through hooks. The `react-apollo` integration is already declared in `package.json` .

### Adding countdown component to product page

In this practice, we'll modify our Countdown component to search for the targetDate of the releaseDate field of a VTEX product.

Basically, we'll add the countdown component to `./store-theme/store/blocks/product.jsonc` (right before the `buy-button` ).

After doing this preparation, let's start our GraphQL queries.

### Realease Date Query

We'll store the query in a new file: `./react/querires/productReleaseDate.graphql` .

In this example, the query will receive the product slug as the term. Then, the resolver `product` will be called and will retrieve the specified field(s).

``` graphql
query productReleaseDate($slug: String){
	  product(slug: $slug) {
		    releaseDate
	  }
}
```

The `product` resolver is already available through the `vtex.search-graphql` app.

Now we need to declare our dependencies in theme's `manifest.json` :

``` json
"dependencies": {
  "vtex.styleguide": "9.x",
  "vtex.css-handles": "0.x",
  "vtex.search-graphql": "0.x",
  "vtex.product-context": "0.x"
}
```

And we also need to import the `useQuery` , `useProduct` hooks - as well as our `productReleaseDate` query. So our `Countdown.tsx` file imports will be like this:

``` tsx
import React, { useState } from 'react'
import { TimeSplit } from './typings/global'
import { tick } from './utils/time'
import { useCssHandles } from 'vtex.css-handles'
import { useQuery } from 'react-apollo'
import useProduct from 'vtex.product-context/useProduct'
import productReleaseDate from './queries/productReleaseDate.graphql'
```

> It is important to higlight that there is the possibility of your IDE showing an error while importing product-context.

### Defining the query

We can define the query using the productReleaseDate imported and the useQuery hook, like this:

``` tsx
const { product: { linkText } } = useProduct()

const { data, loading, error } = useQuery(productReleaseDate, {
  variables: {
    slug: linkText
  },
  ssr: false
})
```

We must deal with cases in which there is no data (error) or before receiving the return (loading):

``` tsx
if (loading) {
  return (
    <div>
      <span>Loading...</span>
    </div>
  )
}
if (error) {
  return (
    <div>
      <span>Error!</span>
    </div>
  )
}
```

### Passing releaseDate to tick function

Finally, we can remove the component's props and add the releaseDate in tick function:

``` tsx
tick(data?.product?.releaseDate || DEFAULT_TARGET_DATE, setTime)
```

_Note that the question mark makes property 'optional' - in cases where there is no such prop, undefined will be returned._

And that's all for today!

_#JahBless_
