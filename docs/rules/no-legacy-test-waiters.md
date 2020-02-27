# (no-legacy-test-waiters)

Prevents the use of the legacy test waiter APIs.

## Rule Details

The legacy test waiters API has been superseded by the new [ember-test-waiters](https://github.com/emberjs/ember-test-waiters) addon.
This new addon provides an enhanced test waiter with robust debugging capabilities. Please use this in favor of the legacy waiter system.

## Examples

Examples of **incorrect** code for this rule:

```js
import Component from '@ember/component';
import { registerWaiter } from '@ember/test';

let counter = 0;

if (DEBUG) {
  registerWaiter(() => {
    return counter === 0;
  });
}

export default Component.extend({
  init() {
    counter++;
    someAsync()
      .then(() => console.log('hi'))
      .finally(() => counter--);
  },
});
```

```js
import Component from '@ember/component';
import { registerWaiter, unregisterWaiter } from '@ember/test';

let counter = 0;
let waiter = () => {
  return counter === 0;
};

if (DEBUG) {
  registerWaiter(waiter);
}

export default Component.extend({
  init() {
    counter++;
    someAsync()
      .then(() => console.log('hi'))
      .finally(() => counter--);
  },

  willDestroy() {
    unregisterWaiter(waiter);
  },
});
```

Examples of **correct** code for this rule:

```js
import Component from '@ember/component';
import { buildWaiter } from 'ember-test-waiters';

let waiter = buildWaiter('my-waiter');

export default Component.extend({
  init() {
    let token = waiter.beginAsync();

    someAsync()
      .then(() => console.log('hi'))
      .finally(() => waiter.endAsync(token));
  },
});
```

## References

For more information on the new test waiters API, please visit [ember-test-waiters](https://github.com/emberjs/ember-test-waiters).