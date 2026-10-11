# Personal plugins for EqyCord

The official installer contains the maintained plugin catalog. Personal plugins use the Vencord-style source build workflow: add your own plugin to `src/userplugins`, then compile your copy. The official client does not load arbitrary JavaScript files from a settings folder.

## Get the source

Download `EqyCord-source.zip` from the same [official release](https://github.com/zztwi/EqyCord-releases/releases) as your installed version and extract it into a permanent folder. Access to the private development repository is not required. Keep the included licenses and notices.

Install Node.js 24 and the pnpm version specified by `packageManager` in `package.json`, then run this inside the extracted folder:

```sh
pnpm install --frozen-lockfile
```

## Add your plugin

Create `src/userplugins/myPlugin/index.ts` (or `index.tsx` for a plugin containing JSX). Plugin names must be unique across the catalog. Use the included plugins as API examples; follow their licenses when reusing code. A small entry point looks like this:

```ts
import definePlugin from "@utils/types";

export default definePlugin({
    name: "MyPersonalPlugin",
    description: "A plugin for my own EqyCord build.",
    authors: [{ name: "YourName", id: 123456789012345678n }],
    start() {
        // Start your feature here.
    },
    stop() {
        // Remove listeners and undo your feature here.
    }
});
```

Replace the example author ID with your own Discord ID. Existing Vencord-compatible plugins may work, but API changes, dependencies and duplicate names can require changes. They are not automatically verified by EqyCord.

## Build and use your copy

For desktop:

```sh
pnpm buildStandalone
```

On Windows, if you already have an official EqyCord installation, use its setup's **Restore Discord** first. Close Discord completely, including the tray icon, then run:

```sh
node scripts/eqycord/installer.mjs install --branch stable
```

Use `canary` or `ptb` instead of `stable` for those clients. Keep the source folder in place: the desktop loader uses its built files. Open Discord, find your plugin in Plugins, and enable it. Patch-based plugins may require another restart.

For a browser build:

```sh
pnpm buildWeb
```

Load the generated `dist/chromium-unpacked` extension folder in a supported Chromium browser's extension developer mode, using a separate profile from your official EqyCord Web installation.

## Updating personal builds

Official automatic updates are disabled for source-archive and personal-plugin builds, so an official update cannot erase your custom plugins. To update, extract a newer source archive, copy your `src/userplugins` folder into it, install dependencies and rebuild. Check your plugins against the new version before using them.

Official builds continue to use the public update channel. A plugin being available in a new release does not automatically enable an existing plugin that you previously disabled.
