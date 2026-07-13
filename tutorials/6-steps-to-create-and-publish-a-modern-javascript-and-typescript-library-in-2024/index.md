# 6 Steps to Create and Publish a Modern Javascript and Typescript Library in 2024

All you need to create and share your Javascript or Typescript library with the Open-source community.

## Prerequisites

Make sure that Node.js and NPM are installed on your computer before you start
this step-by-step guide. To publish your library, you will also need an NPM user
account.

Git and GitHub are not necessary, but you will need them if you want to share
your library with other developers and let them contribute to the source code of
your library.

## Step 1: Lay a strong foundation with a solid scaffolding

It is important to build a strong foundation for the code before diving into it.

At this stage, we usually do a lot of research on how other libraries organize
their code, find the best ways to do things, and test tools for making bundles
and setting up live servers.

Even though Vite is fairly new to the JavaScript ecosystem — it was made in 2020
— it has quickly become popular among front-end developers and for good reason.

Some of the great things about Vite are its fast live server, pre-configured
bundles, and many other features. In addition, it can easily set up a simple but
strong structure for your library. You can quickly get your library up and
running by executing the command below and following the on-screen instructions.

```sh
npm create vite
```

## Step 2: Understand the structure of your library

### The root directory

The name of the root directory is the name you choose for your library.

### Node modules directory

NPM installs all of the dependencies your library needs in the “node_modules”
directory.

Under “devDependencies” and “dependencies” in the “package.json” file, you can
find all the dependencies your library needs.

### The public directory and index.html file

The “public” directory and “index.html” file are only used to test your library
through the browser when you run the following command to turn on the live
server:

```sh
npm run dev
```

### Source code directory

All the source code for your library will go in the source directory. It could
be TypeScript, JavaScript, CSS, SVG, or something else.

A few files for the demo app that Vite makes when you set up the library are
already in the source directory. Only the “vite-env.d.ts” file is not for this
demo app. It is a TypeScript declaration file, you can set up global variables
and types for your library.

### The .gitignore file

The “.gitignore” file tells Git which files and directories the version control
system (VCS) should not look at.

### The package.json and package-lock.json files

The “package.json” file is a metadata file that NPM uses to describe things
about a project, like scripts, dependencies, versions, and so on.

The “package-lock.json” file, on the other hand, is made when we install the
dependencies in a JavaScript project. It contains the exact version of each
dependency that is installed.

### tsconfig.json

The “tsconfig.json” is used to set the compiler options in TypeScript projects.
It lets you set things like the target ECMAScript version, the module system,
and the paths that should be compiled or not compiled.

## Step 3: Get things cleaned up

When the scaffolding is made, Vite adds a demo app by default. Let’s get rid of
everything that has to do with this demo app.

First, let’s get rid of the “vite.svg” file that is in the “public” directory.
You could even get rid of the whole “public” directory if you do not need any
assets to test your library.

Let’s remove all the files that are in the “src” directory. Only “main.ts” could
be kept because we can erase its content and write our code in it.

## Step 4: Write and test your library

Now that you understand the structure of your library and have gotten rid of all
the files that are not needed, it is time to get to work and write the code.

To give you a practical example, I created a library that converts values to and
from percentages. Here is a sneak peek at what this library might look like:

```ts
function toPercentage(value: number, total: number): number {
  return (value / total) * 100;
}

function fromPercentage(percentage: number, total: number): number {
  return (percentage / 100) * total;
}

export { toPercentage, fromPercentage };
```

Using a live server is one way to make sure that your code works right. I
changed the “index.html” file in my library so it looks like this:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Percentage</title>
  </head>

  <body>
    The percentage of 10 out of 100 is <span id="percentage"></span>
    <script type="module">
      import { toPercentage } from "./src/main";
      document.getElementById("percentage").innerText = toPercentage(10, 100);
    </script>
  </body>
</html>
```

Try the following command to see if your library works as it should:

```sh
npm run dev
```

## Step 5: Build your library

The build process will transform your source code into a format that can be
easily used in any JavaScript environment.

One of the scripts that Vite adds automatically to your “package.json” file is
the build script. All you have to do is run the following command:

```sh
npm run build
```

When you run the build command, this is what you see:

```sh
✓ 4 modules transformed.
dist/index.html 0.35 kB               │ gzip: 0.25 kB
dist/assets/index-CdKwhZ4x.js 0.80 kB │ gzip: 0.45 kB
✓ built in 96ms
```

Vite created the “dist” directory and put an “index.html” and a JavaScript file
with a random name in the “dist/assets/” directory.

It is fine to use this build for regular projects, but not for libraries. Let’s
make a Vite configuration file that tells Vite how to build a library.

Create the _vite.config.ts_ file in the root directory with the following
content:

```ts
import { defineConfig } from "vite";

export default defineConfig({
  build: {
    lib: {
      entry: "src/main.ts",
      name: "percentage",
      fileName: "percentage",
    },
  },
});
```

The above configuration specifies that we want to build only the “main.ts” file
and the name of the generated files.

Please keep in mind that “pctage” is the name I chose for my library. Instead,
you should use the name of your library.

If you run the build command again, this is what you should see:

```sh
✓ 1 modules transformed.
dist/pctage.js 0.14 kB      │ gzip: 0.11 kB
dist/pctage.umd.cjs 0.39 kB │ gzip: 0.26 kB
✓ built in 69ms
```

Now, Vite created two files in the dist directory. The “pctage.js” file is in
CommonJS format for Node.js environments, and the “pctage.umd.cjs” file is in
UMD format for browser environments.

## Step 6: Publish your library

Make sure that only the important files are published so that your library stays
small and easy to use. By making a “.npmignore” file, you can tell NPM which
files it should not publish.

In the root directory, create the “.npmignore” file with the following content:

```sh
public/
src/
index.html
vite.config.ts
```

Make sure you change the “private” property in your “package.json” to “false” if
you want the library to be open to everyone.

You should also add the “main” property to your “package.json”. For example:

```json
{
  //...
  "main": "pctage.js",
  "private": false
  //...
}
```

This way, when someone else runs `require('your-package')`, they will get
whatever your "main" module exports.

Also, make sure you include important details like the description, keywords,
homepage, bugs, and license information. To find out more, read the
[official documentation](https://docs.npmjs.com/cli/v10/configuring-npm/package-json).

If you are not already logged into your NPM account on your computer, you will
need to run the command below to access NPM. You will be asked to enter your
username and password for your NPM account when you run this command.

```sh
npm adduser
```

Following the Semantic Versioning rules, you should raise the version number
every time you need to publish your library. In your “package.json” file, you
need to change the version number to reflect whether the changes are major,
minor, or patch updates.

```sh
npm publish
```

---

## Conclusion

At first, it might seem hard to create and publish your own library. But it is a
lot easier to handle if you break the process down.

To make something truly valuable, you need to do research. Look through existing
libraries to get ideas and learn the best ways to do things.

I really want to see what you have created. Leave a comment below with the
libraries you have published.

Happy coding!

