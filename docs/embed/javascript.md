---
title: Sheet music Embed JavaScript client
description: Interact with our Sheet Music Embed, Get user events from our viewer and editor
permalink: embed/javascript.html
pid: embed-js
---

Use this JavaScript Client to interact and receive events from our [Sheet Music Embed](https://flat.io/developers/embed).

This API requires an API Key, [please contact us for more information](mailto:developers@flat.io).

## Installation

You can install our Embed Client using npm or bower:

```bash
npm install flat-embed
```

or

```bash
bower install flat-embed
```

## Getting Started

The simplest way to get started is the pass a DOM element to our embed that will be used as container. By default, this one will completely fit its container:

```html
<div id="embed-container"></div>
<script src="./embed.min.js"></script>
<script>
  var container = document.getElementById('embed-container');
  var embed = new Flat.Embed(container, {
    score: '<score-id-you-want-to-load>',
    embedParams: {
      appId: '<your-app-id>',
      controlsFloating: false
    }
  });
</script>
```

## Embed construction

### DOM element or existing iframe

When instantiating `Flat.Embed`, the first argument will always refer to a DOM element. It can take:

* A DOM element (e.g. selected using `document.getElementById('embed-container')`).
* The string identifier of the element (e.g. `"embed-container"`).
* A jQuery element (e.g. selected using `$('#embed-container')`). If multiple elements match the selection, the client will take the first one selected.
* An existing embed iframe element. In this case, this one will need to have our JS API loaded using the query string [`jsapi=true`](https://flat.io/developers/docs/embed/url-parameters.html).

If you instance different `Flat.Embed` for the same element, you will always get the same instance of the object.

### Options and URL parameters

When instantiating `Flat.Embed`, you can pass options in the second parameter. In order to use the different methods available and events subscriptions, you will need to pass at least `embedParams.appId`.

| Option | Description | Values | Default |
|:-------|:------------|:-------|:-------|
| `score` | The score identifier that will load initially | Unique score id | `blank` |
| `width` | The width of your embed | A width of the embed | `100%` |
| `height` | The height of your embed | A height of the embed | `100%` |
| `embedParams` | Object containing the loading options for the embed | [Any URL parameters](https://flat.io/developers/docs/embed/url-parameters.html) | `{}`

## JavaScript API

* [Methods](#methods)
  * [`ready`](#ready-promisevoid-error): Wait until the JavaScript is ready
  * [`on`](#onevent-string-callback-function-void): Subscribe to events
  * [`off`](#offevent-string-callback-function-void): Unsubscribe from events
  * [`loadFlatScore`](#loadflatscoreid-string-promisevoid-apierror): Load a score hosted on Flat
  * [`loadMusicXML`](#loadmusicxmlscore-mixed-promisevoid-error): Load MusicXML file (compressed or not)
  * [`loadJSON`](#loadjsonscore-object-promisevoid-error): Load Flat JSON file
  * [`getMusicXML`](#getmusicxmloptions-object-promisestringuint8array-error): Get the score in MusicXML (compressed or not)
  * [`getJSON`](#getjson-object): Get the score data in Flat JSON format
  * [`getScoreMeta`](#getscoremeta-object): Get the metadata from the current score (for hosted scores)
  * [`fullscreen`](#fullscreenstate-bool-promisevoid-error): Toggle fullscreen mode
  * [`play`](#play-promisevoid-error): Start playback
  * [`pause`](#pause-promisevoid-error): Pause playback
  * [`stop`](#stop-promisevoid-error): Stop playback
  * [`print`](#print-promisevoid-error): Print the score
  * [`getZoom`](#getzoom-promisenumber-error): Get the current display zoom ratio
  * [`setZoom`](#setzoomnumber-promisenumber-error): Change the display zoom ratio
  * [`getAutoZoom`](#getautozoom-promiseboolean-error): Get the state of the auto-zoom mode
  * [`setAutoZoom`](#setautozoomboolean-promiseboolean-error): Enable or disable the auto-zoom mode
* [Events API](#events-api)
  * [`scoreLoaded`](#event-scoreLoaded): A new score has been loaded
  * [`cursorPosition`](#event-cursorposition): The cursor position changed
  * [`rangeSelection`](#event-rangeSelection): The range selected changed
  * [`fullscreen`](#event-fullscreen): The fullscreen state changed
  * [`print`](#event-print): The score was printed
  * [`play`](#event-play): The score playback started
  * [`pause`](#event-pause): The score playback paused
  * [`stop`](#event-stop): The score playback stopped
  * [`playbackPosition`](#event-playbackposition): The playback slider position changed

## Methods

You can call the methods using any `Flat.Embed` object. By default, the methods (except `on` and `off`) return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) that will be resolved once the method is called, the value is set or get:

```js
var embed = new Flat.Embed('container');
embed.loadFlatScore('12234').then(function () {
  console.log('Score loaded');
}).catch(function (err) {
  console.log('Error', err);
});
```

### `ready(): Promise<void, Error>`

Promises resolved when the embed is loaded and the JavaScript API is ready to use. All the methods will implicitly use this method, so you don't have to worry about waiting the loading before calling the different methods.

```js
embed.ready().then(function() {
 // You can use the embed
});
```

### `on(event: string, callback: function): void`

Add an event listener for the specified event. When receiving the event, the client will call the specified function with zero or one parameters depending of the [event received](#events-api).

```js
embed.on('playbackPosition', function (position) {
  console.log(position);
});
```

### `off(event: string, callback?: function): void`

Remove an event listener for the specified event. If no `callback` is specified, all the listener for the event will be removed.

```js
function positionChanged(position) {
  // Print position
  console.log(position);

  // You can remove the listener later (e.g. here, once the first event is received)
  embed.off('play', positionChanged);

  // Alternatively, you can remove all the listeners for the event:
  embed.off('play');
};

// Subscribe to the event
embed.on('positionChanged', positionChanged);
```

### `loadFlatScore(id: string): Promise<void, ApiError>`

Load a score hosted on Flat using its identifier. For example to load `https://flat.io/score/56ae21579a127715a02901a6-house-of-the-rising-sun`, you can call:

```js
embed.loadFlatScore('56ae21579a127715a02901a6').then(function () {
  // Score loaded in the embed
}).catch(function (error) {
  // Unable to load the score
});
```

### `loadMusicXML(score: mixed): Promise<void, Error>`

Load a MusicXML score, compressed (MXL) or not (plain XML):

```js
fetch('https://api.flat.io/v2/scores/56ae21579a127715a02901a6/revisions/last/mxl')
.then(function (response) {
  return response.arrayBuffer();
})
.then(function (mxl) {
  return embed.loadMusicXML(mxl);
})
.then(function () {
  // Score loaded in the embed
})
.catch(function (error) {
  // Unable to load the score
});
```

### `loadJSON(score: object): Promise<void, Error>`

Load a score using Flat's JSON Format

```js
fetch('https://api.flat.io/v2/scores/56ae21579a127715a02901a6/revisions/last/json')
.then(function (response) {
  return response.json();
})
.then(function (json) {
  return embed.loadJSON(json);
})
.then(function () {
  // Score loaded in the embed
})
.catch(function (error) {
  // Unable to load the score
});
```

### `getMusicXML(options: object): Promise<string|Uint8Array, Error>`

Convert the current displayed score into a MusicXML file, compressed (`.mxl`) or not (`.xml`).

```js
// Uncompressed MusicXML
embed.getMusicXML().then(function (xml) {
  // Plain XML file (string)
  console.log(xml);
});
```

Example: Retrieve the score as a compressed MusicXML, then convert it to a Blob and download it:
```js
// Uncompressed MusicXML
embed.getMusicXML({ compressed: true }).then(function (buffer) {
  // Create a Blob from a compressed MusicXML file (Uint8Array)
  var blobUrl = window.URL.createObjectURL(new Blob([buffer], {
    type: 'application/vnd.recordare.musicxml+xml'
  }));

  // Create a hidden link to download the blob
  var a = document.createElement('a');
  a.href = blobUrl;
  a.download = 'My Music Score.mxl';
  document.body.appendChild(a);
  a.style = 'display: none';
  a.click();
  a.remove();
});
```

### `getJSON(): object`

Get the data of the score in the "Flat JSON" format (a MusicXML-like as a JavaScript object).

```js
embed.getJSON().then(function (data) {
  console.log(data);
}).catch(function (error) {
  // Unable to get the data
});
```

### `getScoreMeta(): object`

Get the score metadata of the hosted score. The object will have the same format that the one returned [by our API `GET /v2/scores/{score}`](https://flat.io/developers/api/reference/#operation/getScore).

```js
embed.getScoreMeta().then(function (metadata) {
  console.log(metadata);
}).catch(function (error) {
  // Unable to get the metadata
});
```

### `fullscreen(state: bool): Promise<void, Error>`

Display the embed in fullscreen (`state = true`) or return to the regular display (`state = false`).

```js
embed.fullscreen(true).then(function () {
  // The score is now displayed in fullscreen
});
```

### `play(): Promise<void, Error>`

Load the playback and play the score.

```js
embed.play().then(function () {
  // The score is playing
});
```

### `pause(): Promise<void, Error>`

Pause the playback

```js
embed.pause().then(function () {
  // The playback is paused
});
```

### `stop(): Promise<void, Error>`

Stop the playback

```js
embed.stop().then(function () {
  // The playback is stopped
});
```

### `print(): Promise<void, Error>`

Print the score

```js
embed.print().then(function () {
  // The score is being printed (the browser opens the print prompt)
}).catch(function (error) {
  // Error when printing
});
```

### `getZoom(): Promise<number, Error>`

Get the current zoom ration applied for the score (between 0.5 and 3).

```js
embed.getZoom().then(function (zoom) {
  // The zoom value
  console.log(zoom);
});
```

### `setZoom(number): Promise<number, Error>`

Set a new zoom ration for the display (between 0.5 and 3).

```js
embed.setZoom(2).then(function (zoom) {
  // The zoom value applied
  console.log(zoom);
});
```

### `getAutoZoom(): Promise(<boolean, Error>)`

Get the state of the auto-zoom mode. Auto-zoom is enabled by default for page mode or when the [URL parameter `zoom`](https://flat.io/developers/docs/embed/url-parameters.html) is set to `auto`.

This getter will return `true` if the auto-zoom is enabled, and `false` when disabled. Setting a zoom value with [`setZoom`](#setzoomnumber-promisenumber-error) will disable this mode.

```js
embed.getAutoZoom().then(function (state) {
  // The auto-zoom state
  console.log(state);
});
```

### `setAutoZoom(boolean): Promise(<boolean, Error>)`

Enable (`true`) or disable (`false`) the auto-zoom. Auto-zoom is enabled by default for page mode or when the [URL parameter `zoom`](https://flat.io/developers/docs/embed/url-parameters.html) is set to `auto`. Setting a zoom value with [`setZoom`](#setzoomnumber-promisenumber-error) will disable this mode.

```js
embed.setAutoZoom(false).then(function (state) {
  // Auto-zoom mode is disabled
  console.log(state);
});
```

## Events API

Events are broadcasted following actions made by the end user or you with the JavaScript API. You can subscribe to an event using the method [`on`](#onevent-string-callback-function-void), and unsubscribe using [`off`](#onevent-string-callback-function-void). When an event includes some data, this data will be available in the first parameter of the listener callback.

### Event: `scoreLoaded`

This event is triggered once a new score has been loaded. This event doesn't include any data.

### Event: `cursorPosition`

This event is triggered when the position of the user's cursor changes.

```json
{
    "partIdx": 0,
    "staffIdx": 1,
    "voiceIdx": 0,
    "measureIdx": 2,
    "noteIdx": 1
}
```

### Event: `rangeSelection`

This event is triggered when a range of notes is selected or the selection changed.

```json
{
    "from": {
        "partIdx": 0,
        "measureIdx": 1,
        "staffIdx": 0,
        "voiceIdx": 0,
        "noteIdx": 2
    },
    "to": {
        "partIdx": 0,
        "measureIdx": 3,
        "staffIdx": 0,
        "voiceIdx": 0,
        "noteIdx": 5
    }
}
```

### Event: `fullscreen`

This event is triggered when the state of the fullscreen changed. The callback will take a boolean as the first parameter that will be `true` if the fullscreen mode is enabled, and `false` is the display is back to normal (fullscreen exited).

### Event: `print`

This event is triggered when you or the end-user prints the score. This event doesn't include any data.

### Event: `play`

This event is triggered when you or the end-user starts the playback. This event doesn't include any data.

### Event: `pause`

This event is triggered when you or the end-user pauses the playback. This event doesn't include any data.

### Event: `stop`

This event is triggered when you or the end-user stops the playback. This event doesn't include any data.

### Event: `playbackPosition`

This event is triggered when the playback slider moves. It is usually triggered at the beginning of every measure and will contain an object with information regarding the position of the playback in the score:

```json
{
    "beat": "4",
    "beatType": "4",
    "tempo": 120,
    "currentMeasure": 1,
    "timePerMeasure": 2
}
```