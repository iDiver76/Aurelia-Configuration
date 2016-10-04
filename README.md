# aurelia-configuration

This Repository is a fork from [Vheissu/aurelia-configuration](https://github.com/Vheissu/aurelia-configuration)

## Install aurelia-configuration

```bash
jspm install github:iDiver76/aurelia-configuration
```

* Add plugin to your app's main.js:

```javascript
export function configure(aurelia) {
    aurelia.use
        .standardConfiguration()
        .developmentLogging()
        .plugin('aurelia-configuration');

    aurelia.start().then(a => a.setRoot());
}
```

* Use the plugin to **set** configuration in your app's main.ts:

```javascript
import {Configure} from "aurelia-configuration";
// [...]
aurelia.use
        .standardConfiguration()
        .plugin('aurelia-i18n', (instance) => {
                let configInstance = aurelia.container.get(Configure);
                let apiEndpoint = configInstance.get('api.endpoint');
```

* Create a config file. By default the plugin will assume a configuration file called: config.json inside of a root directory called "config" - the contents of the JSON file can be anything you like as long as it is a JSON object. You can configure the plugin to use a different config file if you like.

```json
{
    "name": "Test Application",
    "version": "1.2",
    "api": {
        "key": "somekey",
        "endpoint": "http://www.google.com/"
    }
}
```

**Using with your ViewModel:**

```javascript
import {inject} from 'aurelia-framework';
import {Configure} from 'aurelia-configuration';

@inject(Configure)
export class ViewModel {
    constructor(config) {
        this.config = config;

        // Get configuration data using this.config
        // Single non-namespace item:
        this.config.get('name') // Using above sample config would return 'Test Application'
        // Single namespaced item:
        this.config.get('api.key') // Using above sample config would return 'somekey'
            
        // Get config item and if it doesn't exist return a default value provided as the second argument
        this.config.get('name', 'default value');

        // Setting a temporary config value non-namespaced:
        this.config.set('newkey', 'surprise!') // Would store a value of 'surprise!' on object {newkey: 'surprise!'}
        // Setting a temporary config value namespaced:
        this.config.set('websites.name', 'Google'); // Would store a value of 'Google' on object {websites: {name: 'Google'}}
    }
}
```