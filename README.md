# How to make ESLint work with Prettier avoiding conflicts and problems

([post published here](https://dev.to/pixari/how-to-make-eslint-work-with-prettier-avoiding-conflicts-and-problems-57pi))

Having [prettier](https://prettier.io/) and [ESLint](https://eslint.org/) up and running on your project can be very useful and can save us a lot of time by identifying static errors as we introduce them into the code and assure a consistent style to your files.

Prettier is a famous "code formatter" which ensures that all outputted code conforms to a consistent style.

ESLint is a JavaScript linting utility which performs static analysis in order to find problematic patterns or code that doesn't adhere to certain style guidelines.

##Prettier and ESlint, two hearths one love
It is very common nowadays to use them both at the same time.
Unfortunately, it is very easy to have configuration errors that often generate really annoying conflicts.

In this post I try to give a couple of indications to configure VSCode properly and avoid problems and conflicts.

## Prerequisites
 -[Visual Studio Code](https://code.visualstudio.com/)
 -[VS Code ESLint plugin](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
  
## What to do
1. First of all you have to **install ESLint plugin** in VS Code.  Either you can use the **extension** tab in VS Code or just the links provided in the “Prerequisites” section of this post.
2. Then you have to install in your project **Prettier** and **ESLint** as node modules:

```bash
npm install --save-dev eslint prettier
```

3. Now it’s time to create a config file for ESLInt:

```bash
./node_modules/eslint/bin/eslint.js --init
```

Or if you installed it globally you can use:

```bash
eslint --init 
```

Pick the following options:
* To check syntax, find problems, and enforce code style
* JavaScript modules (import/export)
* None of these
* TypeScript: No
* Browser or Node, as you prefer
* Use a popular style guide
* Airbnb (personally I really like this style guide)
* JavaScript
* Yes (install all dependencies)

After this process, you’ll find a new file in your root folder: **.eslintrc.js**

My file looks like this:
```js
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: [
    ‘airbnb-base’,
  ],
  parserOptions: {
    ecmaVersion: 12,
    sourceType: ‘module’,
  },
  rules: {
  },
};
```

This is the **config file for ESLint**.

4. Now it’s time to create a config file for prettier:

```js 
// .prettierrc.js
module.exports = {
  trailingComma: "es5",
  tabWidth: 2,
  semi: true,
  singleQuote: true,
};

```

One of the most common problem people are experiencing with Prettier/ESLint is having conflicting warnings and lot of red lining errors.

A good way to avoid this problem is using Prettier as a ESLint plugin.

That’s why you have to install a special plugin called “**eslint-plugin-prettier**” ad dev dependency:

```bash
npm install --save-dev eslint-plugin-prettier
```

Once it’s installed, you have to tell ESLint to use Prettier **as a plugin**:

```js 
module.exports = {
  env: {
    es6: true,
    browser: true,
    es2021: true,
  },
  extends: [‘airbnb-base’],
  parserOptions: {
    ecmaVersion: 12,
    sourceType: ‘module’,
  },
  rules: {
    ‘prettier/prettier’: ‘error’,
  },
  plugins: [‘prettier’],
};

```

At this point, you have both Prettier and ESLint up and running on your code.

Even if it’s working, it could be that some rules will conflict.
To avoid this problem,  you have to turns off all rules that are unnecessary or might conflict with Prettier.

```bash
npm install --save-dev eslint-config-prettier
```

Once it’s installed, you have to update your **.eslintrc.js** file:

```js 
module.exports = {
  env: {
    es6: true,
    browser: true,
    es2021: true,
  },
  extends: ['airbnb-base', 'prettier'],
  parserOptions: {
    ecmaVersion: 12,
    sourceType: 'module',
  },
  rules: {
    'prettier/prettier': 'error',
  },
  plugins: ['prettier'],
};

```

## Yeah! You did it!
In your project now you have ESLint and Prettier working perfectly at the same time.
Prettier runs as a plugin of ESLint and thanks to the special configuration it won’t conflict with it.

You can check a working example on this repo: [GitHub - codejourneys-org/eslint-prettier](https://github.com/codejourneys-org/eslint-prettier)

If you have any questions please do not hesitate to leave a comment or join a great community of Front-End developers :smiley: : [CodeJourneys.org](https://codejourneys.org)
