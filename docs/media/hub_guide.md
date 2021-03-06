---
---
# Hub

AWS Amplify has a lightweight Pub-Sub system called Hub. It is used to share data between modules and components with the help of event subscriptions.

## Installation

Import:
```js
import { Hub } from 'aws-amplify';
```

## Working with the API

### dispatch()

You can dispatch an event with `dispatch` function:
```js
Hub.dispatch('auth', { event: 'signIn', data: user }, 'Auth');
```

### listen()

You can subscribe to a channel with `listen` function:
```js
import { Hub, Logger } from 'aws-amplify';

const logger = new Logger('MyClass');

class MyClass {
    constructor() {
        Hub.listen('auth', this, 'MyListener');
    }

    // Default handler for listening events
    onHubCapsule(capsule) {
        const { channel, payload } = capsule;
        if (channel === 'auth') { onAuthEvent(payload); }
    }
}
```

In order to capture event updates, you need to implement `onHubCapsule` handler function in you listener class.
{: .callout .callout--info}

### Listening Authentication Events

AWS Amplify Authentication module publishes in `auth` channel when 'signIn', 'signUp', and 'signOut' events happen. You can create your listener to listen and act upon those event notifications.

```js
import { Hub, Logger } from 'aws-amplify';

const logger = new Logger('MyClass');

class MyClass {
    constructor() {
        Hub.listen('auth', this, 'MyListener');
    }

    onHubCapsule(capsule) {
        const { channel, payload } = capsule;
        if (channel === 'auth') { onAuthEvent(payload); }
    }

    onAuthEvent(payload) {
        const { event, data } = payload;
        switch (event) {
            case 'signIn':
                logger.debug('user signed in');
                break;
            case 'signUp':
                logger.debug('user signed up');
                break;
            case 'signOut':
                logger.debug('user signed out');
                break;
            case 'signIn_failure':
                logger.debug('user sign in failed');
                break;
        }
    }
}
```

### API Reference

For the complete API documentation for Hub module, visit our [API Reference]({%if jekyll.environment == 'production'%}{{site.amplify.baseurl}}{%endif%}/api/classes/hubclass.html)
{: .callout .callout--info}
