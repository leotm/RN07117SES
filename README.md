# ReadMe

cc @erights https://github.com/endojs/endo/pull/2206#discussion_r1560094895

Follow https://reactnative.dev/docs/0.71/environment-setup <br>
_React Native CLI Quickstart > macOS > Android_ <br>
to get setup to run the app on an emulator

Run `yarn` in a terminal window

Edit with e.g. VSCode <br>
`code node_modules/react-native/Libraries/Core/InitializeCore.js`

```js
// ...

'use strict';

require('../../../../ses.cjs');

repairIntrinsics({ errorTaming: 'unsafe', consoleTaming: 'unsafe', errorTrapping: 'none', unhandledRejectionTrapping: 'none', overrideTaming: 'severe', stackFiltering: 'verbose' });
// hardenIntrinsics(); // TODO, app currently hangs when enabled

const start = Date.now();

// ...
```

Run `yarn start` to boot Metro

Then type `a` to boot up the app on an Android emulator

The app should be functional, since parts of `ses.cjs` are temporarily commented with TODO's (see commits)

Adding a `debugger` statement without Flipper running will hang the app execution

To debug, download and open `Flipper-mac.dmg` from <br>
https://github.com/facebook/flipper/releases/tag/v0.125.0

On first open, you may need to allow Flipper to run via Mac `Privacy & Security > Security` system settings

Open `Hermes Debugger` (which should connect to Metro) to hook into a `debugger` statement and breakpoints should work <br>
(You may need to type `a` again to rebuild the app, then re-open Hermes Debugger within Flipper for successful debugging)
_Nb: Couldn't get Hermes Debugger to work testing in RN 72/73, i can create repo's for these too to demonstrate_

In `ses.cjs`, uncomment any of these TODO's to repro issues listed in https://github.com/endojs/endo/issues/1891

```js
// ...

// assertDirectEvalAvailable(); // TODO

// ...

  addIntrinsics(tameFunctionConstructors());

  addIntrinsics(tameDateConstructor(dateTaming));
  addIntrinsics(tameErrorConstructor(errorTaming, stackFiltering));
  addIntrinsics(tameMathObject(mathTaming));
  addIntrinsics(tameRegExpConstructor(regExpTaming)); // Fixed in: https://github.com/endojs/endo/pull/2108
  addIntrinsics(tameSymbolConstructor()); // Fixed in: https://github.com/endojs/endo/pull/2206 and this PR

// addIntrinsics(getAnonymousIntrinsics()); // TODO

// completePrototypes(); // TODO

// ...

// whitelistIntrinsics(intrinsics, markVirtualizedNativeFunction); // TODO

// ...
```

Commits: https://github.com/leotm/RN07117SES/commits/main
