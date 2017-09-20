<template>
  <div id='app'>
    <h2>Todos Again - vue.js by郭诗语</h2>
    <input id='add-input' v-model="newItem" v-on:keyup.enter="addNew" placeholder='输入待办事项'/>
    <ul>
      <li v-for='item in items'>
        <h3 @mouseenter='itemEnter(item)' @mouseleave='itemLeave(item)'>
          <input type="checkbox" @click='itemCheck(item)'>
          <p class="item-label" v-bind:class="{'line-through':item.checked}">{{ $index }}.{{item.label}}</p>
          <p calss='item-status' v-if='item.checked'>finished</p>
          <p class='item-delete' v-if='item.showDelete' @click='deleteClick(item)'>Delete</p>
        </h3>
      </li>
    </ul>
  </div>
</template>

<script>
import Store from './store'
export default {
  data () {
    return {
      items: Store.fetch(),
      newItem: ''
    }
  },
  watch: {
    items: {
      handler: function () {
        Store.save(this.items)
      },
      deep: true
    }
  },
  methods: {
    addNew: function () {
      this.items.push({
        label: this.newItem,
        checked: false,
        showDelete: false
      })
      this.newItem = ''
    },
    itemCheck: function (item) {
      item.checked = !item.checked
    },
    itemEnter: function(item){
      item.showDelete = true;
    },
    itemLeave: function(item){
      item.showDelete = false;
    },
    deleteClick: function(item){
      this.items.$remove(item);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body {
  font-family: Helvetica,sans-serif;
}
#app {
  width: 800px;
  margin:30px auto;
}
#app-input {
  width: 750px;
  height: 35px;
  padding: 0 5px;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  height: 30px;
}
.item-status {
  display: inline;
  background: red;
  color: white;
  padding: 0 5px;
  font-size: 12px;
}
.item-delete {
  display: inline;
  text-decoration: underline;
  font-size: 12px;
  color: gray;
  cursor: pointer;
}
.item-label {
  display: inline;
}
.line-through {
  text-decoration: line-through;
}
</style>
