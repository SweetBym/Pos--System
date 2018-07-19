<template>
  <div id="app">
      <v-leftNav></v-leftNav>

      <div class="main">
          <router-view></router-view>
      </div>
  </div>
</template>

<script>

    import leftNav from '@/components/common/leftNav.vue'
export default {

    name: 'App',
    components:{
      'v-leftNav': leftNav
    }
}
</script>

<style>
    .main{
        float:left;
        width:95%;
        background-color: #EFF2F7;
        height:100%;
        overflow: auto;
    }
</style>
