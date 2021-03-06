# Mutation Writing Tutorial
This is a tutorial on how to write a simple mutation for aurora
## npm
First you’ll need to install node if you don’t already have it. That can be done with this link: https://nodejs.org/en/download/
Node comes with a version of npm, but you will need to check if it is the latest version. That can be done with entering this command in your terminal
`npm install npm@latest -g`
To test if it updated enter the command
`npm -v `
The version should be higher than 2.1.8

Once you have npm downloaded and updated you can then you can start creating your mutation. Navigate to where you would like your npm package to be stored. Then create a folder called `aurora-mutate-nameOfMutation`. Enter the following command into your command line
`npm init`
![npm init]
(https://imgur.com/rEk9owK)
This command creates a package.json file and it will prompt you to enter in values for the fields of the `package.json`. The default values are fine to use.
![package json](https://imgur.com/4kMpiWm)
## Mutation
After you create the package.json file, then you can create your mutation. Create another file in the same folder that your `package.json` is in. It needs to be titled the same as what you put in the `entry point` field when creating your `package.json`. In this running example that file is `index.js`.
![mutation setup](https://imgur.com/Sy2hIXm)
Your mutation will need to be written in React, which is a framework for javascript. If you do not already have an editor that works well with React, you can get Atom or VScode.
https://atom.io/
https://code.visualstudio.com/

Here is an example on how to mutate the editor of Aurora to turn it purple
```import React from "react";
import styled from "styled-components";
import Editor from "draft-js";

const purple = `
  color: #8B008D;
`;

function purpleEditor(Editor) {
  return class extends React.Component {
    render() {
      return (
        <Editor>
          <purple />
          {this.props.children}
        </Editor>
      );
    }
  };
}

module.exports.mutations = {
  Editor: purpleEditor
};
```
Here is an example on how to mutate the frame of Aurora to replace it with a flying cat gif
```import React from "react";
import styled from "styled-components";

const EVERYWHERE = styled.img`
  position: fixed;
  top: 0;
  left: 0;
  min-width: 100%;
  min-height: 100%;
`;

function AddKitty(Frame) {
  return class extends React.Component {
    render() {
      return (
        <Frame>
          <EVERYWHERE src="https://media.giphy.com/media/VxbvpfaTTo3le/giphy.gif" />
          {this.props.children}
        </Frame>
      );
    }
  };
}

module.exports.mutations = {
  Frame: AddKitty
};
```
The import statements used are to import react and styled components. React is the language used to write the mutation and styled components allow us to change how the components(Editor and Frame) look. The constants (`const`) are used to style the components using css. If you want a more in depth guide for writing React check this out
https://reactjs.org/docs/hello-world.html
For more information about the React technique used to write mutations check this out https://reactjs.org/docs/higher-order-components.html


## Webpack
Webpack is a bundler, and we’re going to use it to bundle your mutation once you have written it. You can install Webpack by entering this onto your command line
`npm install webpack -g`
Next you’ll have to create a webpack.config.js file. This file will tell webpack what it should do. Here is an example of what it should look like.
```
const path = require("path");

module.exports = {
  entry: "./index.js", //tells webpack where to start
  output: {
    filename: "./build/bundle.js", //tells webpack the output
    libraryTarget: "commonjs2"
  },
  module: {
    loaders: [
      { test: /\.js$/, loader: "babel-loader", exclude: /node_modules/ },
      { test: /\.jsx$/, loader: "babel-loader", exclude: /node_modules/ }
    ]
  }
};
```
Next, you’ll have to install some babel dependencies by running the following command on your command line
`npm install --save-dev babel-core babel-preset-env babel-preset-react  babel-loader`
Then, you’ll have to bundle in react and styled components by running the following command
`npm install --save react styled-components`

Your `package.json` file should now look something like this
```
{
  "name": "aurora-mutate-purple-editor",
  "version": "1.0.0",
  "description": "Turns the aurora editor purple",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "aurora",
    "mutation"
  ],
  "author": "rainmaker",
  "license": "ISC",
  "dependencies": {
    "react": "^16.1.0",
    "styled-components": "^2.2.3",
    "webpack": "^3.8.1"
  },
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.2",
    "babel-preset-env": "^1.6.1",
    "babel-preset-react": "^6.24.1"
  }
}
```
After you write your config file and run the previous npm commands, run the command `webpack` on your command line. This will create a build folder and a bundle.js file.
![webpack](https://imgur.com/zszzTEC)
## Publishing your npm package
To publish a npm package you have to register yourself as a user. If you have already created your account on the npm website use the command `npm login` to login to your account. If you have not created an account yet use the command `npm adduser` to create your npm account. The next step is to publish your npm package by using the command `npm publish`. This command will publish everything in the current directory, so make sure you are in the right spot before running this command.
![npm publish](https://imgur.com/m4lkqRA)
After publishing your package you can also update it by using the `npm version <update-type>` command, where `<update-type>` is either patch, major, or minor.
