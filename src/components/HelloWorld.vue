<template>
  <div class="hello">
    <h1 :style="{color:colorArray[colorIndex]}">{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      colorArray: ["red", "blue", "green"],
      colorIndex: 0
    }
  },
  watch: {
    colorIndex() {
      if (this.colorIndex==3) {
        this.colorIndex=0
      } else {
        setTimeout(() => this.colorIndex++, 1000)
      }
    }
  },
  mounted() {
    setTimeout(() => this.colorIndex++, 1000)
  }
} 
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
