# Electron React Template
This project is the base template for developing Electron applications using React.

# React Scripts
- react-start
- react-build
- react-test
- react-eject

# Electron Scripts
- electron-build
- release
- build: create an executable version of the application based on your OS.
- start: use to run a demo

# Reproducce the Template
Follow steps below in order to reproduce the same template.

1. Create a React App
```shell
npx create-react-app <your_app_name> --typescript
```
Note: Recent version of Electron builder has TS dependency. Hence, Typescript option is required.

2. Installing Dependencies
```shell
# yarn
yarn add cross-env electron-is-dev
yarn add --dev concurrently electron electron-builder wait-on
# npm
npm install --save cross-env electron-is-dev
npm install concurrently electron electron-builder wait-on
```

3. Create Electron App
Create a file named `electron.js` in public folder and paste code below inside the file.

```js
const electron = require("electron");
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;
const path = require("path");
const isDev = require("electron-is-dev");

let mainWindow;

function createWindow() {
mainWindow = new BrowserWindow({ width: 900, height: 680 });
    mainWindow.loadURL(isDev ? "http://localhost:3000": 
        `file://${path.join(__dirname, "../build/index.html")}`);

    mainWindow.on("closed", () => (mainWindow = null));
}

app.on("ready", createWindow);

app.on("window-all-closed", () => {
    if (process.platform !== "darwin") {
        app.quit();
    }
});

app.on("activate", () => {
    if (mainWindow === null) {
        createWindow();
    }
});
```

4. Update package.json
Add following information to `package.json`

```json
{
    "description": "<your project description>",
    "author": "<author of app>",
    "build": {
    "appId": "<com.your_app>"
    },
    "main": "public/electron.js",
    "homepage": "./",
    "scripts": {
        "react-start": "react-scripts start",
        "react-build": "react-scripts build",
        "react-test": "react-scripts test --env=jsdom",
        "react-eject": "react-scripts eject",
        "electron-build": "electron-builder",
        "release": "npm run build && electron-builder --linux --win --mac --x64 --ia32 --publish never",
        "build": "npm run react-build && npm run electron-build",
        "start": "concurrently \"cross-env BROWSER=none yarn react-start\" \"wait-on http://localhost:3000 && electron .\""
  },
}
```

5. Run the Project

```shell
#  ==== Runing ====
# yarn
yarn start
#npm
npm run start

# ==== Building ====
# yarn
yarn build
# build
npm run build
```


# Acknowledgement
This template and the reproduction manual were taken from [Devesu](https://medium.com/@devesu) article on Medium. [Link to Article](https://medium.com/@devesu/how-to-build-a-react-based-electron-app-d0f27413f17f)