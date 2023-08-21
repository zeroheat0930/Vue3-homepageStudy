<script setup>
// eslint-disable-next-line no-undef,no-unused-vars
const props = defineProps({
  msg: String
})

// eslint-disable-next-line no-undef
const emit = defineEmits(['response'])
emit('response', '자식 컴포넌트로부터 🌷를 받았어요!')
</script>

<template>
  <h2>{{ msg || 'prop이 아직 전달되지 않았습니다!' }}</h2>
  <h2>자식 컴포넌트</h2>
</template>