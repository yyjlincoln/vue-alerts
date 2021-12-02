<p align="center" style="font-size: 3em;">Vue-Alerts</p>

<p align="center">
<a href="https://badge.fury.io/js/vue-alerts"><img src="https://badge.fury.io/js/vue-alerts.svg" alt="npm version" height="18"></a>
  <br>

</p>

<p>Vue-Alerts is a Vue 2.x plugin that delivers UIViewController-like alerts with customisable alert actions and more. Check out the demo <a href="https://yyjlincoln.github.io/vue-alerts-demo">here</a>.</p>

## Installing Vue-Alerts

Vue-Alert is published on [npm](https://www.npmjs.com/package/vue-alerts). You can add it to your project via:

### yarn
```bash
yarn add vue-alerts
```

### npm

```bash
npm install vue-alerts
```

## Using Vue-Alerts

To use Vue-Alerts, you'll need to import the plugin.

In `main.js`, or the JavaScript where initialised Vue, add:

```javascript
import Vue from 'vue' // Should have already been imported in the file.
import vueAlerts from "vue-alerts"
```

Then, register the plugin with Vue:

```javascript
Vue.use(vueAlerts, options)
```
Once that's done, specify a root Vue Model for the alerts. Please note that you may only register one root VM per application so we would recommend that to be the root VM of your app, i.e. in `App.vue`:

```html

<div id="app">

  <!-- BEGIN INSERT -->
  <alert></alert>
  <!-- END INSERT -->

  <router-view></router-view>
  
</div>
```
You're all set! Now you can refer to the module through:

```javascript
this.$alert
```

in any VM.

## Presenting an alert with `async present()`

### **Parameters**

| #   | Parameter    | Type          | Required | Default                                |
| --- | ------------ | ------------- | -------- | -------------------------------------- |
| 0   | title        | String        | Yes      |                                        |
| 1   | message      | String        | Yes      |                                        |
| 2   | actions      | Array[Object] | No       | An OK button that dismissess the alert |
| 3   | < Multiple > | Object        | No       | {}                                     |

#### **Additional Parameters**

Those configurations can be passed into the function via #3. They're essentially named keyword arguments.

You typically don't need to worry about those additionally configurations.

| Name                | Type    | Required | Default | Description                                                                                                              |
| ------------------- | ------- | -------- | ------- | ------------------------------------------------------------------------------------------------------------------------ |
| defaultAction       | Integer | No       | null    | The index of action when enter is pressed. If null, then it will become the first action with type `cancel`. See below.  |
| defaultEscapeAction | Integer | No       | null    | The index of action when escape is pressed. If null, then it will become the first action with type `cancel`. See below. |
| preventKeyboard     | Boolean | No       | true    | Whether we would prevent the keyboard from interaction with the elements on the page                                     |
| allowMultipleClicks | Boolean | No       | false   | Allows the user to click an option (or multiple options) multiple times before it's handled.                             |
| immediately         | Boolean | No       | false   | Whether the alert should be presented immediately. If false, the alert will be placed in a presentation queue.           |

### **Resolution**

`present()` will never reject. It resolves after the alert is presented, as it moves to the front of the presentation queue.

When it resolves, it will yield an integer alert identifier. With the identifier, you can:

- Access the alert info
```javascript
this.$alert.alerts[identifier]
```
> *It's read-only*

- Dismiss the alert.

> *Documented below. Check out dismiss()*

### **Example**

  ```javascript
  this.$alert.present("Vue-Alerts Message", "Helloworld!", [
    {
      title: "OK",
      type: "cancel",
    }
  ], {
    preventKeyboard: false
  })
  ```
*For more examples, check out the [demo](https://yyjlincoln.github.io/vue-alerts-demo)*

### **Defining the Actions**

The actions parameter is an array of objects that defines the type of action and its handler.

#### **General Structure**
```
actions = [
  { action },
  { action },
  ...
]
```

#### **The Action Object**

For each action object, you can define the following properties:

| Name    | Type     | Required | Default | Description                                                                                                                             |
| ------- | -------- | -------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| title   | String   | Yes      |         | The name of the action (button)                                                                                                         |
| type    | String   | Yes      |         | This can be one of: cancel, normal or destructive                                                                                       |
| handler | Function | Depends  |         | For action with type `cancel`, the handler will never be called. For all other types, a handler will be required. More explained later. |



### **Handling Action Calls**

When a user clicks on an action, the handler will be called with the following parameters:

| #   | Type    | Description                                     |
| --- | ------- | ----------------------------------------------- |
| 0   | Integer | The identifier of the alert                     |
| 1   | Integer | The index of the action, as defined in actions. |

The handler can be either async or sync. In either case, the handler should return a boolean value indicating whether the alert should be automatically dismissed.

#### **Example**

```javascript
async function handler(){
  let response = await SomeAPIRequest()
  if(response.status === true) {
    console.log("Success")
    // return true is not neccessary here. It dismissess by default.
  } else {
    this.$alert.present("Request Failed", response.message)
    return false // Stops the current alert from dismissing
  }
}

this.$alert.present("Get API?", "Are you sure?", [
  {
    title: "Yes",
    type: "normal",
    handler: handler
  },
  {
    title: "No",
    type: "cancel",
  }
])
```

## Dismissing an alert programatically with `async dismiss()`

Note that, by default, the alert will dismiss after an action is clicked and the handler does not return false. You would not need to call `dismiss()` manually.

However, if for some reasons you want to dismiss the alert programmatically, you can do so by calling `dismiss()`


### **Parameters**

| #   | Parameter    | Type    | Required | Default        |
| --- | ------------ | ------- | -------- | -------------- |
| 0   | identifier   | integer | No       | The last alert |
| 1   | < Multiple > | Object  | No       | {}             |

#### **Additional Parameters**

| Name        | Type    | Required | Default | Description                                                                                           |
| ----------- | ------- | -------- | ------- | ----------------------------------------------------------------------------------------------------- |
| immediately | Boolean | No       | false   | Whether the alert should be presented immediately. If false, the dismissal will be placed in a queue. |

### **Resolution**

`present()` will never reject. It resolves after the alert is dismissed with a boolean value indicating whether or not the alert was dismissed.

## Demo

Check out the demo here:

- [Demo](https://yyjlincoln.github.io/vue-alerts-demo)
- [Demo (Source Code)](https://github.com/yyjlincoln/vue-alerts-demo)

## License

Vue-Alerts is licensed under the [Apache License 2.0
](https://github.com/yyjlincoln/vue-alerts/blob/master/LICENSE).

Copyright @yyjlincoln