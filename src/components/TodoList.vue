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
    <div class="main">
      <ul>
        <li class="big-font"  :key="index" v-for = "(item,index) in topics">{{ item.name }}
          <i class="el-icon-circle-plus" @click="showInputArea(index)"></i>
          <i class="el-icon-circle-close" @click="deleteTopic(index)"></i>
          <el-input v-show="item.showInput" @keyup.enter.native="addThing(index)"
                    placeholder="please input one thing todo"
                    v-model="newThing">
          </el-input>
          <ul>
            <!--<child-list v-for="(thing,index) in item.list"></child-list>-->
            <li class="small-font" :key="index1" v-for="(thing,index1) in item.list" @click="changeState(index,index1)">
              <p :class="thing.state?'class-done':'class-none' ">{{thing.name}}</p>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
// import ChildList from './ChildList'
export default {
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
        // 先判断topic是否重名
        let isOnly = true
        for (let i = 0; i < this.topics.length; i++) {
          if (this.topics[i].name === this.newTopic) {
            isOnly = false
          }
        }
        if (isOnly) {
          let topics = JSON.parse(storage.topic)
          storage.setItem('topic', JSON.stringify(topics.concat({name: this.newTopic, showInput: false, list: []})))
          console.log(window.localStorage)
        } else {
          alert('this topic has existed')
        }
      } else {
        storage.setItem('topic', JSON.stringify([].concat({name: this.newTopic, showInput: false, list: []})))
        console.log(window.localStorage)
      }
      this.topics = JSON.parse(window.localStorage.topic)
      this.newTopic = ''
    },
    deleteTopic (index) {
      this.topics.splice(index, 1)
      this.updateLocalStorage()
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
  .main{
    width:450px;
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
  }
  ul{
    margin: 0 auto;
  }
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
