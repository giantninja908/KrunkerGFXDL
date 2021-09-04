# Krunker GFXDL

this is a utility for krunker users to do the stuffz


## Get started

Before using KGFXDL please see [Tauri Introduction](https://tauri.studio/en/docs/getting-started/intro) and follow instructions to setup your environment.

Install the dependencies...

```bash
yarn
```

...then start development server:

```bash
yarn tauri dev
```

This will take care of running both frontend and backend of your app with watch attached to both. That means whenever you change something in `src` (svelte frontend code) or `src-tauri` (rust backend code), it will be automatically processed and hot reloaded. To finish dev/debug mode simply close the app window.

## Building and running in production mode

To create an optimised version of the app:

```bash
yarn tauri build
```

This will create standalone app and installer in `src-tauri/target/release` directory.

## Useful links

-   [Tauri](https://tauri.studio)
-   [Svelte](https://svelte.dev)
