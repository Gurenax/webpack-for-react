# Webpack for React
- instructions referenced from https://medium.freecodecamp.org/learn-webpack-for-react-a36d4cac5060
- about babel presets/plugins: https://www.fullstackreact.com/articles/what-are-babel-plugins-and-presets/
- about property initializers: https://www.fullstackreact.com/articles/use-property-initializers-for-cleaner-react-components/
- about react hot reloading: https://gaearon.github.io/react-hot-loader/getstarted/

## Install React, React Router, Semantic UI for React
```
yarn add react react-dom react-prop-types react-router-dom semantic-ui-react
```

## Install Babel Modules, CSS Modules, Webpack Modules
```
yarn add babel-core babel-loader babel-preset-env babel-preset-react babel-preset-stage-1 css-loader style-loader webpack-contrib/html-webpack-plugin webpack webpack-dev-server -D
```

## Install Webpack CLI
```
yarn add webpack-cli -D
```

## Setup Babel
```
# In root directory
touch .babelrc
```

`.babelrc`
```
{
  "presets": ["env", "react", "stage-1"]
}
```
* "env" - Tells babel we are using ES2015, ES2016 and ES2017 all together
* "react" - Tells babel we are using React
* "stage-1" - Tells babel we are Stage 1 (TC39 Category - Proposal)
* These presets will tell `babel-loader` what to do


## Setup Webpack
```
# In root directory
touch webpack.config.js
```

`webpack.config.js`
```javascript
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')

const port = process.env.PORT || 3000

module.exports = {
  // Webpack configuration goes here
}
```

## Configure Webpack
`webpack.config.js`
```javascript
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')

const port = process.env.PORT || 3000

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'bundle.[hash].js'
  },
  devtool: 'inline-source-map',
  module: {
    rules: [
      // First Rule - ESNext (Babel)
      {
        test: /\.(js)$/,
        exclude: /node_modules/,
        use: ['babel-loader'] // Loads .babelrc
      },

      // Second Rule - CSS Modules
      {
        test: /\.css$/,
        use: [
          {
            loader: 'style-loader'
          },
          {
            loader: 'css-loader',
            options: {
              modules: true,
              camelCase: true,
              sourceMap: true
            }
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: 'public/index.html',
      favicon: 'public/favicon.ico'
    })
  ],
  devServer: {
    host: 'localhost',
    port: port,
    historyApiFallback: true,
    open: true,
    hot: true // Enables hot-reloading
  }
}
```

## Add scripts in package.json
```javascript
"scripts": {
  "start": "webpack-dev-server"
}
```

## Create the React App

1. Create `public` folder
```
# In root directory
mkdir public && cd $_
touch index.html
```

2. Add `favicon.ico` inside public folder

3. Copy the following to `index.html`
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.2.13/semantic.min.css"></link>
  <title>webpack-for-react</title>
</head>

<body>
  <div id="root"></div>
</body>

</html>
```

4. Create `index.js`
```
# In root directory
mkdir src && cd $_
touch index.js
```

`index.js`
```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import App from './components/App'

ReactDOM.render(<App />, document.getElementById('root'))
```

5. Create `components` directory and the components
```
# In src directory
mkdir components && cd $_
touch App.js Layout.js Layout.css Home.js DynamicPage.js NoMatch.js
```

`App.js`
```javascript
import React from 'react'
import { Switch, BrowserRouter as Router, Route } from 'react-router-dom'

import Home from './Home'
import DynamicPage from './DynamicPage'
import NoMatch from './NoMatch'

const App = () => {
  return (
    <Router>
      <div>
        <Switch>
          <Route exact path="/" component={Home} />
          <Route exact path="/dynamic" component={DynamicPage} />
          <Route component={NoMatch} />
        </Switch>
      </div>
    </Router>
  )
}

export default App
```

`Layout.css`
```css
.pull-right {
  display: flex;
  justify-content: flex-end;
}

.h1 {
  margin-top: 10px !important;
  margin-bottom: 20px !important;
}
```

`Layout.js`
```javascript
import React from 'react'
import { Link } from 'react-router-dom'
import { Header, Container, Divider, Icon } from 'semantic-ui-react'

import { pullRight, h1 } from './layout.css'

const Layout = ({ children }) => {
  return (
    <Container>
      <Link to="/">
        <Header as="h1" className={h1}>
          webpack-for-react
        </Header>
      </Link>
      {children}
      <Divider />
      <p className={pullRight}>
        Made with <Icon name="heart" color="red" /> by Esau Silva
      </p>
    </Container>
  )
}

export default Layout
```

`Home.js`
```javascript
import React from 'react'
import { Link } from 'react-router-dom'

import Layout from './Layout'

const Home = () => {
  return (
    <Layout>
      <p>Hello World of React and Webpack!</p>
      <p>
        <Link to="/dynamic">Navigate to Dynamic Page</Link>
      </p>
    </Layout>
  )
}

export default Home
```

`Dynamic.js`
```javascript
import React from 'react'
import { Header } from 'semantic-ui-react'

import Layout from './Layout'

const DynamicPage = () => {
  return (
    <Layout>
      <Header as="h2">Dynamic Page</Header>
      <p>This page was loaded asynchronously!!!</p>
    </Layout>
  )
}

export default DynamicPage
```

`NoMatch.js`
```javascript
import React from 'react'
import { Icon, Header } from 'semantic-ui-react'

import Layout from './Layout'

const NoMatch = () => {
  return (
    <Layout>
      <Icon name="minus circle" size="big" />
      <strong>Page not found!</strong>
    </Layout>
  )
}

export default NoMatch
```


## Start the webpack dev server
```
yarn start
```


# Extras

## Install Webpack Dashboard
1. Install
```
yarn add webpack-dashboard -D
```

2. Configure `webpack.config.js`. Add the following imports
```javascript
// Import the plugin:
const DashboardPlugin = require('webpack-dashboard/plugin');

// If you aren't using express, add it to your webpack configs plugins section:
plugins: [
    new DashboardPlugin()
]

// If using a custom port
plugins: [
    new DashboardPlugin({ port: 3001 })
]

// If you are using an express based dev server, add it with compiler.apply
compiler.apply(new DashboardPlugin());
```

3. Add script to `package.json`
```javascript
"scripts": {
  "dev": "webpack-dashboard -- webpack-dev-server"
}
```

4. Use `yarn dev` instead of yarn start