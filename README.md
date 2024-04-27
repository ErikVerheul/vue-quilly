# vue-quilly

[![npm version](https://img.shields.io/npm/v/vue-quilly?logo=npm&logoColor=fff)](https://www.npmjs.com/package/vue-quilly)
[![npm bundle size](https://img.shields.io/bundlephobia/min/vue-quilly)](https://www.npmjs.com/package/vue-quilly?activeTab=code)
[![NPM Type Definitions](https://img.shields.io/npm/types/vue-quilly)](https://www.npmjs.com/package/vue-quilly?activeTab=code)
[![GitHub License](https://img.shields.io/github/license/alekswebnet/vue-quilly)](https://github.com/alekswebnet/vue-quilly?tab=readme-ov-file#license)

Tiny Vue component, that helps to create [Quill v2](https://quilljs.com/) based WYSIWYG editors in Vue-powered apps.
Flexible setup, no styles, ready for further customization.

Default input data format is HTML, but also has [Delta](https://quilljs.com/docs/delta) support - using Quill API and exposed Quill instance.
In short, HTML and Delta inputs works in a same way, you can use one of them or both formats to change editor data model.

It's not a all-in-one solution and requires further Quill configuration.
In other hand, you can build your own editor, that matches your needs, with easy.
No matter if you want to create full-featured editor with all Quill's modules or small custom solution with extra functionality, you can use this package as a base start point.

Run [demo](vue-quilly.vercel.app/), that shows editors, builded upon `QuillyEditor` component. See demo [code](https://github.com/alekswebnet/vue-quilly/blob/main/demo/).

## Features

- Builded on top of [Quill v2](https://github.com/quilljs/quill) and Vue 3
- Uses `quill/core` to prevent importing all Quill modules
- Works with both HTML and Quill Delta format
- Typescript support

## Setup

```bash
npm add quill@2.0.0 vue-quilly
# Or
pnpm add quill@2.0.0 vue-quilly
```

## Get started

Import Quill full build if you need all modules or [core build](https://quilljs.com/docs/installation#core-build) with minimum required modules:

```ts
import Quill from 'quill' // Full build
import Quill from 'quill/core' // Core build
import { QuillyEditor } from 'vue-quilly'
```

Add core styles. Also import one of Quill's [themes](https://quilljs.com/docs/customization/themes#themes), if you need one:

```ts
import 'quill/dist/quill.core.css' // Required
import 'quill/dist/quill.snow.css' // For snow theme (optional)
import 'quill/dist/quill.bubble.css' // For bubble theme (optional)
```

Define Quill [options](https://quilljs.com/docs/configuration#options):

```ts
const options = {
  theme: 'snow', // If you need Quill theme
  modules: {
    toolbar: true,
  },
  placeholder: 'Compose an epic...',
  readOnly: false
}
```
Initialize the editor:

```ts
const editor = ref<InstanceType<typeof QuillyEditor>>()
const model = ref<string>('<p>Hello Quilly!</p>')
// Quill instance
let quill: Quill | null = null
onMounted(() => {
  quill = editor.value?.initialize(Quill)!
})
```
```html
<QuillyEditor
  ref="editor"
  v-model="model"
  :options="options"
  @text-change="({ delta, oldContent, source }) => console.log('text-change', delta, oldContent, source)"
  @selection-change="({ range, oldRange, source }) => console.log('selection-change', range, oldRange, source)"
  @editor-change="(eventName) => console.log('editor-change', `eventName: ${eventName}`)"
  @focus="(quill) => console.log('focus', quill)"
  @blur="(quill) => console.log('blur', quill)"
  @ready="(quill) => console.log('ready', quill)"
/>
```

Use `v-model` for HTML content type. You can set content in Delta format using Quill instance:

```ts
quill?.setContents(
  new Delta()
    .insert('Hello')
    .insert('\n', { header: 1 })
    .insert('Some ')
    .insert('initial', { bold: true })
    .insert(' ')
    .insert('content', { underline: true })
    .insert('\n')
)
```

Creating editors with `QullyEditor` [demo](https://github.com/alekswebnet/vue-quilly/blob/main/demo/)

## Events

The component emits `text-change`, `selection-change`, `editor-change` events, similar to [Quill events](https://quilljs.com/docs/api#events).

All events types:

| Event name          | Params                                                           |
| ------------------- | ---------------------------------------------------------------- |
| `text-change`       | { delta: `Delta`, oldContent: `Delta`, source: `EmitterSource` } |
| `selection-change`  | { range: `Range`, oldRange: `Range`, source: `EmitterSource` }   |
| `editor-change`     | eventName: `string`                                              |
| `focus`             | quill: `Quill`                                                   |
| `blur`              | quill: `Quill`                                                   |
| `ready`             | quill: `Quill`                                                   |

## License

[MIT](https://choosealicense.com/licenses/mit/)

## Inspiration projects and useful links

https://github.com/quilljs/quill

https://github.com/surmon-china/vue-quill-editor

https://github.com/vueup/vue-quill

https://www.matijanovosel.com/blog/making-and-publishing-components-with-vue-3-and-vite
