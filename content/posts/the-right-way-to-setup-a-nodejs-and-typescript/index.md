+++
date = '2024-08-05T23:16:37+05:30'
draft = false
title = 'The Right Way to Set Up a Node.js and TypeScript'
summary = 'Learn how to set up a Node.js project with TypeScript for better scalability and maintainability. This guide covers everything from initializing a project with Yarn, installing TypeScript, Nodemon, and Express, configuring tsconfig.json, and running your development server using ts-node efficiently.'
category = 'NodeJS'
tags = ['JavaScript', 'Backend', 'Typescript', 'Project Setup']
comments = true
cover = 'thumbnail.png'
reading_time = true
word_count = false
+++

Setting up a Node.js project with TypeScript can be a game-changer for developers seeking to build scalable, maintainable, and efficient applications. TypeScript's static typing, enhanced tooling, and error-catching capabilities make it an invaluable addition to the Node.js ecosystem. In this guide, we’ll walk you through the best way to set up a Node.js and TypeScript project, covering everything from initialization to configuration and best practices.

Let's begin by initializing a Node.js project using Yarn. This process is similar for npm or other package managers as well.

## Initialize the Project

Open the terminal and run:

```sh
yarn init --yes
```

### Install TypeScript & Nodemon

We need to install TypeScript & Nodemon as development dependencies:

```sh
yarn install -D typescript nodemon
```

### Install Express

Express will handle routing and middleware for our server.

```sh
yarn install express
```

## Set Up Project Structure

Create the following file structure for your project:

```
node-ts-project/
├── src/
│   ├── index.ts
│   ├── server.ts
├── package.json
├── tsconfig.json
```

## Server Setup

Below is a basic Express server in `src/server.ts`:

```ts
import express from "express";

const app = express();

app.get("/", (req, res) => {
  res.send("The Server is Running Fine 🚀");
});

export default app;
```

## Create the Server Entry Point

This file imports the Express app and starts the server on port 3000.

```ts
import app from "./server";

const PORT = 3000;

app.listen(PORT, () => {
  console.info(`Server is running at http://localhost:${PORT} 🚀`);
});
```

## Configuring TypeScript

We need to create a configuration file for TypeScript called `tsconfig.json` in the root of the web server and provide some rules in it.

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules"]
}
```

### Explanation of TypeScript Configuration:
- **`target`**: Specifies the ECMAScript target version (`ES2020` is a good choice for modern JavaScript features).
- **`module`**: Specifies the module system (`commonjs` is standard for Node.js applications).
- **`outDir`**: Directory where the compiled JavaScript files will be placed (`./dist`).
- **`rootDir`**: The root directory of TypeScript source files (`./src`).
- **`strict`**: Enables all strict type-checking options, improving code quality.
- **`esModuleInterop`**: Allows default imports from non-ES modules.
- **`include`**: Specifies which files should be included in the compilation (`src/**/*.ts` includes all TypeScript files in `src`).
- **`exclude`**: Specifies files or directories to exclude from compilation (`node_modules`).

## Running the TypeScript Server

Node.js can only execute JavaScript files, so we have a couple of approaches to run our TypeScript server:

### Approach 1: Compile TypeScript to JavaScript

With this approach, you will compile TypeScript to JavaScript and then run the compiled code. `nodemon` will watch for changes in TypeScript files, compile them, and restart the server.

We need to add the following scripts in the `package.json`:

```json
"scripts": {
  "build": "tsc",
  "start": "node dist/index.js",
  "dev": "nodemon --watch 'src/**/*.ts' --exec 'yarn build && yarn start'"
}
```

Run the server in development mode:

```sh
yarn dev
```

This setup will use `nodemon` to watch TypeScript files, compile them into JavaScript, and restart the server whenever changes are detected.

### Approach 2: Using `ts-node` (Recommended)

This approach is more streamlined as `ts-node` directly runs TypeScript code. `nodemon` will watch the TypeScript files and restart the server without the need for manual compilation.

We need to add a config file called `nodemon.json` for `nodemon`:

```json
{
  "watch": ["src"],
  "ext": "ts,json",
  "ignore": ["src/**/*.spec.ts"],
  "exec": "ts-node ./src/index.ts"
}
```

### Explanation of `nodemon.json`:
- **`watch`**: Specifies the directories or files that `nodemon` should watch for changes (watches all files in `src`).
- **`ext`**: Specifies the file extensions that `nodemon` should monitor (`ts` and `json`).
- **`ignore`**: Specifies files or directories that `nodemon` should ignore (`.spec.ts` files used for tests).
- **`exec`**: Specifies the command to execute when a file change is detected (`ts-node` runs the TypeScript entry point directly).

Then, we add the following scripts to `package.json`:

```json
"scripts": {
  "dev": "nodemon src/index.ts",
  "build": "tsc",
  "start": "node dist/index.js"
}
```

We can run the server by executing the following command:

```sh
yarn run dev
```

That's it for this guide! If you face any problems, feel free to drop a comment. I would love to resolve your query. 🚀
