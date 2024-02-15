<script setup lang="ts">
import { nextTick, ref } from 'vue'

const props = defineProps<{
  content: string
}>()

const isEditable = ref(false)
const input = ref<null | { focus: () => null }>(null)
const EditedContent = ref(props.content)

const ToggleEditable = async () => {
  isEditable.value = !isEditable.value

  await nextTick()
  input.value?.focus()
}
</script>

<template>
  <span>
    <div v-if="!isEditable">
      <span v-on:click="ToggleEditable" class="tools">✏️</span>
      <pre v-on:click="ToggleEditable" v-if="!isEditable">{{ EditedContent }}</pre>
    </div>
    <div v-if="isEditable">
      <span v-on:click="ToggleEditable" class="tools">❌</span>
      <textarea
        ref="input"
        tabindex="0"
        @keydown.esc="ToggleEditable"
        v-model="EditedContent"
        type="text"
      />
    </div>
  </span>
</template>

<style scoped>
.tools {
  position: absolute;
  right: 2em;
}
textarea {
  border-style: none;
}
pre,
textarea,
div {
  width: 100%;
  height: 100%;
}
</style>
