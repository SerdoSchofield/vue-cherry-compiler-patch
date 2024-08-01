# vue-compiler-sfc-cherry-cljs
Currently Vue seems to break when you use `<script lang="cljs">` due to a bug (see [vuejs/core #8368](https://github.com/vuejs/core/issues/8368) and [vitejs/vite-plugin-vite #178](https://github.com/vitejs/vite-plugin-vue/issues/178)). This is just a drop-in replacement for the `compiler.parse` function used in [@vitejs/plugin-vue](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue]).

It could be tweaked to use [Squint](https://github.com/squint-cljs/squint) or probably any other language (like CoffeeScript) which is hitting this issue.

# What it does
It's hacky and I just threw it together, but it works.
It:
- Intercepts Vue SFC files which contain `<script lang="cljs">`
- Strips the script block into three parts:
  - Opening tag
  - Content
  - Closing tag
- Passes the content into `cherry-cljs.compileString()` to compile it to JSX
- Changes the opening tag from `<script lang="cljs">` to `<script lang="jsx">`
- Constructs a new script block using the modified parts
- Replaces the old script block in `input` with the new
- Passes the new input into `vue/compiler-sfc.parse()` and returns

It's hacky, but it works. Hopefully, the bug will be addressed by someone more capable than myself and I can go back to using a Vite plugin to do this.

# Usage

```ts
imoprt vue from '@vitejs/plugin-vue'
import { cherry-parse } from 'vue-cherry-compiler-patch'
import * as compilerSFC from 'vue/compiler-sfc'

export default defineConfig({
  plugins: [
    vue({
      compiler: {
        ...compilerSFC,
        parse: cherry-parse,
      }
    })
  ]
})
```

# Special Thanks

- [Vite](https://vitejs.dev/)
- [@vitejs/plugin-vue](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue])
- [Cherry 🍒](https://github.com/squint-cljs/cherry)
