<template>
  <div class="home">
    <block-lib-com></block-lib-com>
  </div>
</template>

<script>
// @ is an alias to /src
import BlockLibCom from '@/components/BlockLibCom.vue';
export default {
  components: { BlockLibCom },
  name: 'HomeView',
}
</script>
