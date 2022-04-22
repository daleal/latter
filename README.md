<h1 align="center">Loveform</h1>

<p align="center">
  <em>
    The form assembly tool that <strong>won't break your heart</strong> 💔.
  </em>
</p>

**Loveform** is a tool for writing and validating forms when using [**Vue 3**](https://vuejs.org). Unstyled, **Loveform** will only take care of what needs to take care of, and not what it doesn't.

## Installation

Install using npm! (or your favourite package manager)

```sh
# Using npm
npm install loveform

# Using yarn
yarn add loveform
```

## Quick Example

```vue
<script setup lang="ts">
import { ref } from 'vue';
import { LForm, LInput } from 'loveform';

const formRef = ref<LForm | null>(null);

const email = ref('');
const description = ref('');

const emailValidations = [
  (value: string) => !!value.trim() || 'This field cannot be empty',
  (value: string) => value.includes('@') || 'Invalid email',
];

const descriptionValidations = [
  (value: string) => !!value.trim() || 'The description cannot be empty',
];

const submit = () => {
  if (formRef.value?.valid) {
    // do something to submit the form
    // when every input is valid
  }
};
</script>

<template>
  <LForm ref="formRef">
    <LInput
      v-model="email"
      :validations="emailValidations"
    />
    <LInput
      v-model="description"
      :validations="descriptionValidations"
    />
  </LForm>
  <button @click="submit">Submit</button>
</template>
```

## Usage

**Loveform** exports the following components to create forms:

- `LInput`
- `LForm`

### `LInput`

To use the `LInput` component, pass use a `v-model` on it to some reactive value:

```vue
<script setup lang="ts">
import { ref } from 'vue';
import { LInput } from 'loveform';

const inputValue = ref('');
</script>

<template>
  <LInput
    v-model="inputValue"
  />
</template>
```

The `inputValue` variable will update as the user writes on the input.

Each `LInput` also contains a `valid` exposed attribute, which corresponds to the state of the validations for said input. You can access this attribute by setting a `ref` on the input component and then directly calling `.valid`.

```vue
<script setup lang="ts">
import { ref } from 'vue';
import { LInput } from 'loveform';

const inputRef = ref<LInput | null>(null);

const email = ref('');

const emailValidations = [
  (value: string) => !!value.trim() || 'This field cannot be empty',
  (value: string) => value.includes('@') || 'Invalid email',
];

const submit = () => {
  if (inputRef.value?.valid) {
    // do something to submit the input
    // when it is valid
  }
};
</script>

<template>
  <LInput
    v-model="email"
    :validations="emailValidations"
  />
  <button @click="submit">Submit</button>
</template>
```

Notice how we declared the `ref` type as `LInput | null`. You can use the `LInput` type to explicitly declare the type of a template ref.

When calling `inputRef.value?.valid`, you will receive a boolean representing whether the validations set for the input passed or not. Please note that the validations are **watched**, which means that they will run when **something changes** (either the validations themselves or the input value), and not when calling `validate`. This means that you can safely call the `.valid` attribute multiple times in the same method without re-running the validations all over again.

When an error is detected, the error string of the first validation failed will be shown as a `<p>` DOM object under the input. You can hide it by using the `hide-errors` prop on the `LInput` object. There is also an `error` attribute on each input that can be used to manually display the error of said input when the validation fails.

### `LForm`

To use the `LForm` component, render `LInput` components inside of it:

```vue
<script setup lang="ts">
import { ref } from 'vue';
import { LForm, LInput } from 'loveform';

const email = ref('');
const descriptionValue = ref('');
</script>

<template>
  <LForm>
    <LInput v-model="email" />
    <LInput v-model="descriptionValue" />
  </LForm>
</template>
```

The `LForm` component also contains a `valid` exposed attribute, which corresponds to the state of the validations for the inputs inside of said form. You can access this attribute by setting a `ref` on the form component and then directly calling `.valid`.

```vue
<script setup lang="ts">
import { ref } from 'vue';
import { LForm, LInput } from 'loveform';

const formRef = ref<LForm | null>(null);

const email = ref('');
const description = ref('');

const emailValidations = [
  (value: string) => !!value.trim() || 'This field cannot be empty',
  (value: string) => value.includes('@') || 'Invalid email',
];

const descriptionValidations = [
  (value: string) => !!value.trim() || 'The description cannot be empty',
];

const submit = () => {
  if (formRef.value?.valid) {
    // do something to submit the form
    // when every input is valid
  }
};
</script>

<template>
  <LForm ref="formRef">
    <LInput
      v-model="email"
      :validations="emailValidations"
    />
    <LInput
      v-model="description"
      :validations="descriptionValidations"
    />
  </LForm>
  <button @click="submit">Submit</button>
</template>
```

Notice how we declared the `ref` type as `LForm | null`. You can use the `LForm` type to explicitly declare the type of a template ref.

When calling `formRef.value?.valid`, you will receive a boolean representing whether the validations set for **every input inside the form** passed or not.

## Development

### Testing locally

To test locally, first link the library:

```sh
npm link
```

Then, run `npm install` on the `dev` folder and finally start the playground server!

```sh
cd dev
npm install
npm run dev
```

You can see the development server running at `http://localhost:3000`.
