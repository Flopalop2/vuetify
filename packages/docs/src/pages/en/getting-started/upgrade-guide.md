---
emphasized: true
meta:
  nav: Upgrade guide
  title: Upgrade guide
  description: Detailed instruction on how to upgrade Vuetify to 4.0
  keywords: migration, upgrade, releases, upgrading vuetify, alpha, v4
related:
  - /introduction/roadmap/
  - /introduction/long-term-support/
  - /introduction/enterprise-support/
---

# Upgrade Guide

This page contains a detailed list of breaking changes and the steps required to upgrade your application to Vuetify 4

<PageFeatures />

## Styles

- Cascade layers are now being used everywhere. If you have other styles that are not using `@layer` they will now always take priority over vuetify.

## Components

### General changes

#### Slot variables are (mostly) no longer refs

Read-only values passed to slots are now unwrapped:

```diff
  <VForm>
    <template #default="{ isValid, validate }">
      <VBtn @click="validate" text="validate" />
-     Form is {{ isValid.value ? 'valid' : 'invalid' }}
+     Form is {{ isValid ? 'valid' : 'invalid' }}
    </template>
  </VForm>
```

There are still some writable refs though, for example in VDialog:

```html
<VDialog>
  <template #default="{ isActive }">
    <VBtn @click="isActive.value = false">Close</VBtn>
  </template>
</VDialog>
```

Affected components:

- VForm
  - errors
  - isDisabled
  - isReadonly
  - isValidating
  - isValid
  - items

### VBtn

The default text transform of _uppercase_ has been **removed**. To restore the previous behavior, set the `text-transform` prop to `uppercase`.

- Set it in the SASS variables for typography:

```scss
@use 'vuetify/settings' with (
  $typography: (
    'button': (
      'text-transform': 'uppercase',
    ),
  ),
)
```

- Or set it in the SASS variables for buttons:

```scss
@use 'vuetify/settings' with (
  $button-text-transform: 'uppercase',
)
```

- Set it as a global default:

```js
import { createVuetify } from 'vuetify'

const vuetify = createVuetify({
  defaults: {
    VBtn: {
      class: 'text-uppercase',
      // or if you are using $utilities: false
      style: 'text-transform: uppercase;',
    },
  },
})
```

- Manually type uppercase letters:

```diff
- <v-btn>button</v-btn>
+ <v-btn>BUTTON</v-btn>
```

### VSelect/VCombobox/VAutocomplete

#### `item` in slots has been renamed to `internalItem`

For consistency with VList and VDataTable. `item` is still available but is now an alias for `internalItem.raw` which seems like the most common use case.

You can rename:

```diff
  <VSelect item-title="name">
-   <template #item="{ item, props }">
-     <VListItem v-bind="props" :title="item.title" />
+   <template #item="{ internalItem, props }">
+     <VListItem v-bind="props" :title="internalItem.title" />
    </template>
  </VSelect>
```

Or alias:

```diff
  <VSelect>
-   <template #item="{ item, props }">
+   <template #item="{ internalItem: item, props }">
      <VListItem v-bind="props" :title="item.raw.name" />
    </template>
  </VSelect>
```

Or remove `.raw`:

```diff
  <VSelect>
    <template #item="{ item, props }">
-     <VListItem v-bind="props" :title="item.raw.name" />
+     <VListItem v-bind="props" :title="item.name" />
    </template>
  </VSelect>
```
