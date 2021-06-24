Modules allow Quill's behavior and functionality to be customized. Several officially supported modules are available to pick and choose from, some with additional configuration options and APIs. Refer to their respective documentation pages for more details.

To enable a module, simply include it in Quill's configuration.

```javascript
var quill = new DevExpress.Quill('#editor', {
  modules: {
    'history': {          // Enable with custom configurations
      'delay': 2500,
      'userOnly': true
    },
    'syntax': true        // Enable with default configuration
  }
});
```

The [Clipboard](/modules/clipboard.md), [Keyboard](/modules/keyboard.md), and [History](/modules/history.md) modules are required by Quill and do not need to be included explictly, but may be configured like any other module.


## Extending

Modules may also be extended and re-registered, replacing the original module. Even required modules may be re-registered and replaced.

```javascript
var Clipboard = DevExpress.Quill.import('modules/clipboard');
var Delta = DevExpress.Quill.import('delta');

class PlainClipboard extends Clipboard {
  convert(html = null) {
    if (typeof html === 'string') {
      this.container.innerHTML = html;
    }
    let text = this.container.innerText;
    this.container.innerHTML = '';
    return new Delta().insert(text);
  }
}

DevExpress.Quill.register('modules/clipboard', PlainClipboard, true);

// Will be created with instance of PlainClipboard
var quill = new DevExpress.Quill('#editor');
```

*Note: This particular example was selected to show what is possible. It is often easier to just use an API or configuration the existing module exposes. In this example, the existing Clipboard's [addMatcher](/modules/clipboard.md#addMatcher) API is suitable for most paste customization scenarios.*