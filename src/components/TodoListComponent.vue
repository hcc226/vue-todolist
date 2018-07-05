<template>
  <div>
    <el-form :inline="true"  class="demo-form-inline">
      <el-form-item label="Topic">
        <el-input v-model="newTopic" placeholder="input topic name"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="addTopic">ADD</el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="danger" @click="clear">CLEAR</el-button>
      </el-form-item>
    </el-form>
    <ul>
      <li class="big-font" :key="index" v-for = "(item,index) in topics">{{ item.name }}
        <i class="el-icon-circle-plus" @click="showInputArea(index)"></i>
        <el-input v-show="item.showInput" @keyup.enter.native="addThing(index)"
                  placeholder="请输入内容"
                  v-model="newThing">
        </el-input>
        <child-list :things="item.list" :index="index" @change="changeState"></child-list>
          <!--<li class="small-font" v-for="(thing,index1) in item.list" @click="changeState(index,index1)">-->
            <!--<p :class="thing.state?'class-done':'class-none' ">{{thing.name}}</p>-->
          <!--</li>-->
      </li>
    </ul>
  </div>
</template>
<script>
import ChildList from './ChildList'
export default {
  components: {
    ChildList
  },
  data () {
    return {
      newTopic: '',
      topics: [],
      newThing: ''
    }
  },
  methods: {
    addTopic () {
      let storage = window.localStorage
      // storage.clear()
      if (storage.topic) {
        let topics = JSON.parse(storage.topic)
        storage.setItem('topic', JSON.stringify(topics.concat({name: this.newTopic, showInput: false, list: []})))
        console.log(window.localStorage)
      } else {
        storage.setItem('topic', JSON.stringify([].concat({name: this.newTopic, showInput: false, list: ['aaaa']})))
        console.log(window.localStorage)
      }
      this.topics = JSON.parse(window.localStorage.topic)
    },
    clear () {
      this.topics = []
      this.updateLocalStorage()
    },
    updateLocalStorage: function () {
      let storage = window.localStorage
      storage.setItem('topic', JSON.stringify(this.topics))
    },
    showInputArea: function (index) {
      this.topics[index].showInput = true
    },
    addThing: function (index) {
      this.topics[index].list.push({name: this.newThing, state: false})
      this.newThing = ''
      this.topics[index].showInput = false
      this.updateLocalStorage()
    },
    changeState: function (index, index1) {
      this.topics[index].list[index1].state = !this.topics[index].list[index1].state
      this.updateLocalStorage()
    }
  },
  mounted: function () {
    // window.localStorage.clear()
    this.topics = JSON.parse(window.localStorage.topic)
  }
}
</script>
<style scoped>
  li{
    text-align: left;
  }
  .big-font{
    font-size: 22px;
  }
  .small-font{
    font-size: 16px;
  }
  .class-done{
    text-decoration: line-through;
  }
  .class-none{
    text-decoration: none;
  }
</style>
