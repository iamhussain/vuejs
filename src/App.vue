<template>
  <div class="v-container">
      <Plans/>
  </div>
</template>

<script>
import Plans from './components/plans/Plans.vue';


export default {
  name: 'App',
  components: {
    Plans,
  },

  data: () => ({
    //
  }),
};
</script>
<style lang="scss">
@import 'style.scss';
@import url('https://fonts.googleapis.com/css2?family=Poppins&display=swap');
</style>