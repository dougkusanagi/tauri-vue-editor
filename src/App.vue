<template>
  <div class="flex flex-col h-screen items-stretch bg-gray-800">
    <div class="flex text-xs bg-gray-900 text-white/60">
      <button type="button" class="px-2 py-1 hover:bg-white/5">Files</button>
      <button type="button" class="px-2 py-1 hover:bg-white/5" @click="toggleSidebar">Toggle Sidebar</button>
      <button type="button" class="px-2 py-1 hover:bg-white/5">About</button>
    </div>

    <div class="flex flex-1">
      <!-- Sidebar file list -->
      <div v-if="active_sidebar" class="flex flex-col w-56 text-white/70 py-2">
        <h3 class="px-3 mb-2 text-white/40 font-bold">Files</h3>

        <p class="px-3 py-1 text-xs hover:bg-white/5">
          <span class="mr-1">📁</span>
          <span>node_modules</span>
        </p>

        <p class="px-3 py-1 text-xs hover:bg-white/5 bg-[#282C34]">
          <span class="mr-1">📁</span>
          <span>index.html</span>
        </p>
      </div>

      <div class="flex flex-1 pt-2">
        <codemirror
          class="flex-1"
          v-model="code"
          placeholder="Code something amazing!..."
          :style="{ width: '100%', borderTopLeftRadius: '8px', overflow: 'hidden', border: 'none' }"
          :autofocus="true"
          :indent-with-tab="true"
          :tab-size="2"
          :extensions="extensions"
          @ready="handleReady"
          @change="log('change', $event)"
          @focus="log('focus', $event)"
          @blur="log('blur', $event)"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineComponent, ref, shallowRef } from "vue";
import { Codemirror } from "vue-codemirror";
import {Compartment} from "@codemirror/state"
import { javascript } from "@codemirror/lang-javascript";
import { vue } from "@codemirror/lang-vue";
import { html } from "@codemirror/lang-html";
import { oneDark } from "@codemirror/theme-one-dark";

const code = ref(`<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Tauri + Vue 3 App</title>
  </head>

  <body>
    <div id="app"></div>
  </body>
</html>
`)
const languageConf = new Compartment
const extensions = [html(), oneDark];
const active_sidebar = ref(true);

// Codemirror EditorView instance ref
const view = shallowRef();
const handleReady = (payload) => {
  view.value = payload.view
};

// Status is available at all times via Codemirror EditorView
const getCodemirrorStates = () => {
  const state = view.value.state
  const ranges = state.selection.ranges
  const selected = ranges.reduce((r, range) => r + range.to - range.from, 0)
  const cursor = ranges[0].anchor
  const length = state.doc.length
  const lines = state.doc.lines
  // more state info ...
  // return ...
}

function toggleSidebar() {
  active_sidebar.value = !active_sidebar.value;
}
</script>

<style>
html,
body {
  padding: 0;
  margin: 0;
}
</style>
