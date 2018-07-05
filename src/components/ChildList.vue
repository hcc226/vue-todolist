<template>
  <ul>
    <li :key="index" v-for="(thing,index) in things" @click="changeState(index)">
      <p :class="thing.state?'class-done':'class-none' ">{{thing.name}}</p>
    </li>
  </ul>
</template>
<script>
export default {
  name: 'ChildList',
  props: ['things', 'index'],
  data () {
    return {

    }
  },
  methods: {
    getThings: function () {
      return this.things
    },
    changeState: function (index) {
      this.$emit('change', this.index, index)
    }
  }
}
</script>
<style scoped>
  .class-done{
    text-decoration: line-through;
  }
  .class-none{
    text-decoration: none;
  }
</style>
