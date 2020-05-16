<template>
  <div id="app">

    <Header :Hder_pinfor= "Hder_pinfor"></Header>
    <Nav :commentNum='commentNum' ></Nav>
    <router-view />
  </div>
</template>

<script>
  import axios from 'axios'
  import Header from './components/header/header.vue'
  import Nav from './components/nav/nav.vue'
  export default {
    name: 'App',
    components: {
      Header: Header,
      Nav:Nav,

    },
    data() {
      return {
        Hder_pinfor: {},
        commentNum:0
      }
    },
    created() {
      axios.get('/goods').then((res) => {
        this.Hder_pinfor = res.data.data.poi_info
      }),
      axios.get('/ratings').then((res) => {
        this.commentNum = res.data.data.comment_num;
      })
    }
  }
</script>
<style scr='./commom/css/font-awesome.css'></style>
<style>
@import url("./components/header/reset.css")

</style>
