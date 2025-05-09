---
layout: componentLayout
breadcrumb_title: How to Contribute - Flowbite Svelte
title: How to Contribute - Flowbite Svelte
component_title: Contributing
dir: Pages
description: Learn how you can start contributing to the open-source Flowbite Svelte UI component library
---

<script>
  
</script>

Here are some guidelines we'd like you to follow before submitting a PR.

## Create a fork

Create a fork from [Flowbite-Svelte](https://github.com/themesberg/flowbite-svelte) to your repository first.

## Please use pnpm to install

The repo uses `pnpm`, so using `pnpm` is desirable when you fork and install dependencies to avoid unseen problems.

When there is a change in `package.json`, remove `pnpm-lock.yml` and `node_modules` directory and run `pnpm i`.

## Tailwind CSS

[Tailwind warns](https://tailwindcss.com/docs/content-configuration#dynamic-class-names) that you don't construct class names dynamically. Instead of this:

```html
<div class="text-{{ error ? 'red' : 'green' }}-600"></div>
```

Always use complete class names:

```html
<div class="{{ error ? 'text-red-600' : 'text-green-600' }}"></div>
```

## Types and Props

Add a type to `export`-ed variables. When we generate props files, it will automatically pick up types from your file. If you don't add a type, it will break.

Use `lib/types file for nested objects.

```js
// these work
export let icons: AccordionIconType = {
  up: 'chevron-up-solid',
  down: 'chevron-down-solid'
};

export let child: TopMenuType[] | undefined = undefined;
...

// doesn't work
export let comment: CommentType = {
  id: number | string;
  user: {
    name: string;
    joined: string;
  };
};
// put it in lib/types
export interface CommentType {
  id: number | string;
  user: {
    name: string;
    joined: string;
  };
};

// then
export let comment: CommentType;
```

Please run the following to update prop files.

```sh
pnpm gen:docs && pnpm gen:compo-data
```

This first command is to generate component documentation for all Svelte files within the src/lib directory. The second command is to generate JSON files containing props, slots, events information from all Svelte files in the src/lib directory, placing them in the routes/component-data directory.

## Format and check

Please run the following to format the code.

```sh
pnpm format && pnpm check
```

## Conventional commit

When making a commit, we recommend using [the Conventional commits](https://www.conventionalcommits.org/en/v1.0.0/).

Some examples are:

```sh
feat: add rating component
fix: add if statement to Button component
chore: clean up About page
docs: add timeline page
style: update home page
test: add modal test
```

Use `!` for a minor bump.

```sh
feat!: add drawer component and page
```

When you have a breaking change:

```sh
git commit -a "feat: change btnClass name to bClass" -m "BREAKING CHANGE: change the Button component attributes"
```

## Playwright Test

Before submitting a PR, please run a test:

```sh
pnpm test:integration
```

If you want to run an single test file, `tests/typography.spec.ts`:

```sh
npx playwright test tests/typography.spec.ts
```

## A11y Test

We recommend to test a page with [Nu Html Checker](https://validator.unl.edu/) relating to your change.

Test a page.

```sh
axe http://localhost:3000/dropdowns/image
```
