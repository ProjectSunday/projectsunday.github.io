# NPM global installs are an anti-pattern

Globally installing Node Modules is an anti-pattern and should not be encouraged. Here's why, and what to do instead.



## Reasons it's bad

1. Your repo has no control over the globally installed version of the module, meaning if your repo was written a few years ago, the new version users globally install may have unforeseen breaking changes.
1. Alternatively, if your repo is brand new and the user has a very old version of your desired module globally installed, it can also have issues.
1. Users may need two different versions of the same module for two different repos designed to be used with the same globally installed module. Highly inconvenient.
1. Instructions for how to use your repo are not consistent with other repos, slowing users down and requiring additional steps.
1. Globally installing makes it harder to switch dependencies, like from `grunt` to `gulp`, or from `gulp` to `broccoli`, or from `gulp` to `browserify`, or from `gulp` to `webpack`, or from `webpack` to `rollup`. Technology evolves and tastes change. Don't make your users globally install every new dependency you want to try out. If you just update your devDependencies, they won't even notice the changes and will not have to do any extra work to keep your project running locally.
1. No, I will not pollute my systems Environment PATH or global namespace for your project. You are not that important. You are not special. Get off my lawn.



## What to do instead

It's actually surprisingly easy to get around this. We'll be using `gulp` in the following example, because it's pretty popular and the *type* of thing I see being commonly recommended for global installs. But you can mentally replace it with any other Node Module if it makes you feel better.

1. In your `devDependencies` add the module manually and the version number you know works for your project, (hopefully the latest version). The easy way to do this is:
   ```bat
   npm install --save-dev gulp
   ```
1. In the scripts section of your `package.json`, create an alias for whatever the global command you were going to have people run previously.
   ```js
     scripts: {
       "build": "gulp build"
     },
   ```
1. Replace any pre-existing instructions for people to globally install `gulp` with instructions to just `npm install` and then run your new NPM script.
   > **BAD:** To build this repo do `npm install -g gulp` and then run `gulp build`.  
   > **GOOD:** To build this repo do `npm install` and then `npm run build`.



## Reasons why this is good

1. A user should be able to go to any repo with a `package.json` and know they can run the following commands and have the project running.
   * Install Git and Node.js (if not yet installed)
   * `git clone [repo]`
   * `npm install`
   * `npm start`
   * If you have to do more than that, there's a high chance you are doing it wrong.
1. Following this system lowers the barrier of entry for your project, making it easier for potential contributors to work on your project.
1. A user should not have to worry about **your** devDependencies. That's your job as the project creator/maintainer. You have the burden of worry, not the user.
1. You remove many points and possibilities of failure. There are a lot of potential problems that can arise by people having the wrong version of a dependency globally installed.
1. Even if the user already has the dependency globally installed, NPM will default to the locally installed one in the `node_modules` directory.



* * *

That's it, here's some bonus tips for sticking around:

## Bonus NPM tricks

### I have something that needs to be ran *after* the `npm install`

So sometimes you can't just tell a user to `npm install` and `npm start`. What if, for example, you have *additional* dependencies that need downloading from another service, like `bower`?

1. First off, add `bower` as a devDep, don't globally install it (obviously).
1. Create a new NPM script alias called `postinstall`, and assign it the command you want to run after `npm install` finishes. Like so:
   ```js
     "scripts": {
       "postinstall": "bower update"
     },
   ```
1. There is no third step, that's it.

(**Note:** Bower has been deprecated. Move your Bower dependencies over to NPM. If there is a Bower dep that is not on NPM, add an issue on their repo to request they port it over.)

* * *

### How do I make stuff run automatically prior to a commit?

1. `npm install --save-dev pre-commit`
1. In your `package.json` add a section called `pre-commit` and feed it an array of strings that match the aliased names of your NPM scripts:
   ```js
   "pre-commit": [ "lint", "sasslint" ]
   ```
1. Any time you run `git commit -m "Commit Title" -m "Commit Body"` or, if you're lazy, `git commit -m "Did stuff"` it will first run each of the NPM scripts in the pre-commit array, and if nothing throws an error, it will then do the commit.

* * *

### NPM Script naming conventions

Here are the common names I expect for most projects:

* `npm start` - Starts the project, and auto-launches the app or browser
* `npm run serve` - Starts the project, but doesn't auto-launch anything
* `npm run dev` - If `npm start` is for production use, then this is used for development use
* `npm run build` - Builds a package ready for deployment
* `npm run bump` - Bumps the "patch" part of the version number (0.0.1 -> 0.0.2) in `package.json`, the git tag, and pushes
* `npm run deploy` - Builds the package and then deploys it to a network location/server/host
* `npm run test` - Runs some testing suite, typically unit tests
* `npm run lint` - Lints all of your JavaScript files
* `npm run fix` - Lints your JS files and also attempts to auto fix any errors
* `npm run sasslint` - Lints all of your Sass files.
