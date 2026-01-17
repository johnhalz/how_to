# Create Web App Project

## React App
To create a project using:

- React
- typescript (as primary language)
- `pnpm` (as a package manager)
- `swc` (for faster building)

Run the following command in the terminal:

``` bash
pnpm create vite <app_name> --template react-swc-ts
```

## Vue App
To create a project using:

- Vue
- typescript (as primary language)
- `pnpm` (as a package manager)
- `swc` (for faster building)

Run the following command in the terminal:

``` bash
pnpm create vite <app_name> --template vue-ts
pnpm add -D unplugin-swc
```

Then update your `vite.config.ts`:

``` typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import swc from 'unplugin-swc'

export default defineConfig({
  plugins: [
    vue(),
    swc.vite({
      jsc: {
        parser: {
          syntax: 'typescript',
          tsx: false,
        },
        target: 'es2022',
      },
    }),
  ],
})
```

## Tauri App

To create a Tauri app, you only need to run the following command:

``` bash
pnpm create tauri-app
```