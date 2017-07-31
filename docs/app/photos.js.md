---
order: 11
indent: true
---

wq/photos.js
======

[wq/photos.js]

**wq/photos.js** is a [wq/app.js plugin] that integrates with the [PhoneGap Camera API] and shows previews for user-selected photos.  Together with the file processing functions in [wq/app.js] and [wq/outbox.js], wq/photos.js provides a complete solution for allowing volunteers to capture and upload photos via your offline-capable web or mobile app.  Captured photos are saved in an outbox ([wq/outbox.js]) until they can be synchronized to the server.

<div data-interactive id='photo-example'>
  <ul data-role="listview">
    <li class="ui-field-contain">
      <img src="https://wq.io/images/empty.png" id="preview">
      <label for="image">Photo</label>
      <input type="file" name="photos" id="image" data-wq-preview="preview">
    </li>
  </ul>
</div>

The [Species Tracker](http://species.wq.io) application provides a complete demonstration of the offline capabilities of wq/photos.js.

### API
wq/photos.js does not require a global configuration, as it is configured via data-wq- attributes on the related form elements.

```javascript
// myapp/main.js
define(['wq/app', 'wq/photos', './config'],
function(app, photos, config) {

app.use(photos);

app.init(config).then(function() {
    app.jqmInit();
    app.prefetchAll();
});

});
```

To leverage wq/photos.js in your template, create your form elements with data-wq- attributes.  If you are using [wq.start] and the default project layout, these will be set automatically in the generated edit templates for models with image fields.

element | attribute | purpose
--------|-----------|---------
`<input type=file>` | `data-wq-preview` | Indicates the id of an `<img>` element to display a preview image in after a photo is selected.
`<button>` | `data-wq-action` | Indicates the function to call (`take` or `pick`) when the button is clicked
`<button>` | `data-wq-input` | The name of a hidden input to populate with the name of the captured photo.  (The photo itself will be saved in offline storage).
`<input type=hidden>` | `data-wq-type` | Notifies wq/app.js that the hidden element is intended to be interpreted as the name of a photo captured via wq/photos.js.  The element should typically have the `data-wq-preview` attribute set as well.

The `take` and `pick` actions are wrappers for PhoneGap/Cordova's camera.getPicture() API, meant to be used in hybrid apps where <input type=file> doesn't work (e.g. on older devices or broken Android implementations).

Below is an example template with the appropriate attributes set:

```xml
<img id=preview-image>

{{^native}}
<input type=file name=file data-wq-preview=preview-image>
{{/native}}

{{#native}}
<button data-wq-action=take data-wq-input=file>
  Take Picture
</button>
<button data-wq-action=pick data-wq-input=file>
  Choose Picture
</button>
<input id=filename type=hidden name=file
   data-wq-type=file data-wq-preview=preview-image>
{{/native}}
```

Note the use of the `{{#native}}` context flag which is set automatically by [wq/app.js].  See the [Species Tracker template](https://github.com/powered-by-wq/species.wq.io/blob/master/templates/partials/new_photo.html) for a working example.

## Browser Compatibility Notes
wq/photos.js, and the related file processing functions in [wq/app.js] and [wq/outbox.js], rely heavily on two browser features:
 - Offline storage (see Browser Compatibility Notes for [wq/store.js])
 - Binary [Blob] support, including the ability to upload `Blob`s via AJAX.

All modern browsers, including Internet Explorer 10, Android 4.4, and later versions, have at least [minimal blob support](https://github.com/nolanlawson/state-of-binary-data-in-the-browser).  For IE 9 and older desktop browsers, the preview functionality in wq/photos.js will not work, but users should still be able to upload files.  wq/app.js will detect the lack of `Blob` support on these browsers and fall back to a normal form post when a `<form>` containing an `<input type=file>` is encountered.  (wq.db's [ModelViewSet] includes built-in support for responding to forms posted in this way).

[Blob]: https://developer.mozilla.org/en-US/docs/Web/API/Blob
[wq/photos.js]: https://github.com/wq/wq.app/blob/master/js/wq/photos.js
[wq/app.js plugin]: https://wq.io/docs/app-plugins
[wq/app.js]: https://wq.io/docs/app-js
[PhoneGap Camera API]: https://www.npmjs.com/package/cordova-plugin-camera
[camera.getPicture()]: https://www.npmjs.com/package/cordova-plugin-camera
[wq/outbox.js]: https://wq.io/docs/outbox-js
[wq/store.js]: https://wq.io/docs/store-js
[ModelViewSet]: https://wq.io/docs/views
[wq.start]: https://wq.io/wq.start
