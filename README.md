# lerna-test-project
Project to evaluate Lerna

## Results
The combination of Lerna, Webpack, Babel, Jest and ESLint is pretty useful for [PlayCanvas](https://playcanvas.com/) projects.
I will not go into detail for most of these tools. But for a better comprehension, I am providing 
a setup, where these 5 tools are working together.

## Steps
0. Install [Git](https://git-scm.com/) and [NodeJS](https://nodejs.org/en/).
Make sure their commands can be executed in any kind of terminal, command prompt, etc.
1. Install [Visual Studio Code](https://code.visualstudio.com/).
2. Install the the extension called [ESLint](https://eslint.org/) inside Visual Studio Code. To see any effect,
you need to configure ESLint inside your project(s) and you might have to restart Visual Studio Code.
3. Create a new repository on [Github](https://github.com/) or somewhere else.
4. Clone your repository and navigate into it.
5. Open the new folder inside Visual Studio.
6. Open the terminal inside Visual Studio Code: **View > Terminal** From now on, you can enter the following commands inside Visual Studio.
7. If you haven't done it yet, install Lerna globally
```bash
npm install --global lerna
```
8. Create a new lerna repo. I personally prefer the independent versioning mode (--independent)
```bash
lerna init --independent
```
9. Create 2 new packages called math and pc.animation
```bash
cd packages
mkdir math && cd math
npm init -y
cd ..
mkdir pc.animation && cd pc.animation
npm init -y
```
10. Copy and paste the following files into your project:
- [./packages/math/src/inverse-lerp.js](./packages/math/src/inverse-lerp.js)
- [./packages/pc.animation/src/hover-animation.js](./packages/pc.animation/src/hover-animation.js)
- [./packages/pc.animation/src/main.js](./packages/pc.animation/src/main.js)
- [./packages/pc.animation/.babelrc](./packages/pc.animation/.babelrc)
- [./packages/pc.animation/.eslintrc](./packages/pc.animation/.eslintrc)
- [./packages/pc.animation/EXAMPLE.config.json](./packages/pc.animation/EXAMPLE.config.json)
- [./packages/pc.animation/webpack.production.config.babel.js](./packages/pc.animation/webpack.production.config.babel.js)
11. Add the following parts into ./packages/pc.animation/package.json
```json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "cross-env UPLOAD=no NODE_ENV=production webpack --mode production --config webpack.production.config.babel.js"
  },
```
```json
  "devDependencies": {
    "@babel/core": "^7.2.2",
    "@babel/node": "^7.2.2",
    "@babel/plugin-syntax-dynamic-import": "^7.2.0",
    "@babel/preset-env": "^7.2.0",
    "@babel/register": "^7.0.0",
    "babel-core": "^7.0.0-bridge.0",
    "babel-eslint": "^10.0.1",
    "babel-jest": "^23.6.0",
    "cross-env": "^5.2.0",
    "eslint": "^5.10.0",
    "eslint-config-airbnb": "^17.1.0",
    "eslint-loader": "^2.1.1",
    "eslint-plugin-import": "^2.14.0",
    "eslint-plugin-jsx-a11y": "^6.1.2",
    "jest": "^23.6.0",
    "playcanvas-webpack-plugin": "^1.0.8",
    "webpack": "^4.27.1",
    "webpack-cli": "^3.1.2"
  },
```
12. Install required NPM packages
```bash
cd packages/pc.Animation
npm install
```
13. If some packages could not be installed. Run the command above once more!
14. Make **math** available in **pc.animation**
```bash
cd ../..
lerna add math --scope="pc.animation"
```
15. Now you might think, that you are ready to build the project by entering **npm run build** inside packages/pc.Animation.
Unfortunately there are still some things to do. 
For instance, when hovering with your mouse cursor over **math** inside ./packages/pc.animation/package.json, you will see 
that you try to use math version 1.0.0, but the newest version is only 0.0.3. Moreover when analyzing 
./packages/pc.animation/src/hover-animation.js, you will see, that the import of the **inverseLerp** function does not work.
The reason is that, we did not link cross-dependencies yet. Webpack will try to use 
[this math package](https://www.npmjs.com/package/math) instead.
16. To solve this problem, we need to bootstrap the packages inside the Lerna repo
```bash
lerna bootstrap
```
17. Now build your project
```bash
cd packages/pc.Animation
npm run build
```
18. A few seconds later, you should now find a folder called **build** in your pc.Animation package. The build file inside can now be
uploaded into your [PlayCanvas](https://playcanvas.com/) project.

## Tips:
- When you change attributes inside your PlayCanvas scripts, you will need to select the corresponding build file within
your PlayCanvas project and click the **Parse** button.

