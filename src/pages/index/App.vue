<template>
  <div id="app">
    <img src="./assets/logo.png">
    <el-button type="primary" @click="open">主要按钮</el-button>
    
    <router-view/>
  </div>
</template>

<script>
  export default {
    name: 'app',
    methods: {
      open: function () {
        this.$alert('这是一段n内容', '标题', {
          confirmButtonText: '确定',
          callback: action => {
            this.$message({
              type: 'info'
            })
          }
        })
      },
      setVUEx: function () {  
          // this.$store.commit('newAuthor', this.msg);
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

</style>
