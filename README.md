# yarn-hoisting-bug

Demonstrates an issue with Yarn 3.0.2 whereby it does not remove packages that were nohoisted in Yarn 1.

This may be a broader issue than simply hoisting, but that's how I first found it.

## Reproduction

```sh
git checkout 7e6995c0217cdc0c3bdc841aaea64c2ca259f425 # initial commit
yarn install # with 1.22.2 and nohoist rules
git checkout 2f5b633c33e2fa9c759be6d5aedc61b1bdce4298 # latest
yarn install # with 3.0.2 and no hoisting rules
```

At this point you should see that there are `@types/mongoose` directories in both packages as well as the root:

```sh
stat packages/package-a/node_modules/@types/mongoose/package.json
stat packages/package-b/node_modules/@types/mongoose/package.json
stat node_modules/@types/mongoose/package.json
```

If you remove the nohoisted packages and reinstall:

```sh
rm -r packages/package-a/node_modules/@types/mongoose
rm -r packages/package-b/node_modules/@types/mongoose
yarn install
```

They'll stay gone, because they shouldn't be there:

```sh
stat packages/package-a/node_modules/@types/mongoose/package.json # fails
stat packages/package-b/node_modules/@types/mongoose/package.json # fails
```
