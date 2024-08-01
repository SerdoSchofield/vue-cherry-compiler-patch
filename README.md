# vue-compiler-sfc-cherry-cljs
Currently Vue seems to break when you use `&lt;script lang="cljs">` due to a bug. This is just a drop-in replacement for the `compiler.parse` function used in [@vitejs/plugin-vue](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue]).
