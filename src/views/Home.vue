<template>
  <div class="home">
    <SelectBoxNovel />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import SelectBoxNovel from '@/components/SelectBoxNovel.vue'

export default Vue.extend({
  name: 'Home',
  components: {
    SelectBoxNovel,
  },
})
</script>
