<template>
  <v-app>
    <v-main>
      <Index/>
    </v-main>
  </v-app>
</template>

<style>
@import 'https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css';
@import 'https://use.fontawesome.com/releases/v5.0.8/css/all.css';

.v-expansion-panel-content__wrap { 
  padding: 0 !important; 
}

.v-expansion-panel-header {
  padding: 5px !important;
}
.v-alert__icon.v-icon {
  font-size: 28px !important;
  margin-top: 11px !important;
}

.v-alert__content {
  flex: 1 1 auto !important;
  margin-top: 7px !important;
}
</style>

<script>
import Index from './components/Index';

export default {
  name: 'App',

  components: {
    Index,
  },

  data: () => ({
    //
  }),
};
</script>
