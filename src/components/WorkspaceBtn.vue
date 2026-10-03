<template>
  <q-item clickable :to="props.to">
    <q-item-section v-if="props.icon" avatar>
      <q-icon :name="props.icon" />
    </q-item-section>

    <q-item-section>
      <q-item-label class="text-weight-medium">{{ props.label }}</q-item-label>
      <q-item-label v-if="props.caption" caption>{{ props.caption }}</q-item-label>
    </q-item-section>
  </q-item>
</template>

<script setup>
const props = defineProps({
  label: {
    type: String,
    required: true
  },
  caption: {
    type: String,
    default: ''
  },
  // Accepts path '/chat/general' or object { name: 'chat', params: { id: 1 } }
  to: {
    type: [String, Object],
    default: '#'
  },
  icon: {
    type: String,
    default: ''
  }
})
</script>