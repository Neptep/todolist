<template>
  <div id="app">
    <h1 v-text="title">{{title}}</h1>
    <input v-model="newItem" @keyup.enter="addNew">
    <ul>
    	<li v-for="item in items" :class="{finished: item.isFinished}" @click="finish(item)">
    		{{item.label}}
    	</li>
    </ul>
    <component-a msgfromfather='msgOK'></component-a>
  </div>
</template>

<script>
import Storage from './storage'
import ComponentA from './components/componentA'
export default {
  name: 'App',
  components:{ ComponentA },
  data () {
    return {
      title: 'This is a todo list',
      items:Storage.fetch(),
      extraClass: 'liClass',
      newItem:''
    }
  },
  watch:{
  	items:{
  		handler: function(items){
  			Storage.save(items);
  		},
  		deep: true
  	}
  },
  methods:{
  	finish: function(item){
  		item.isFinished = !item.isFinished
  	},
  	addNew: function(){
  		if(this.newItem == '')return false;
  		this.items.push({
  			label: this.newItem,
      	isFinished:false
  		})
  		this.newItem = ''
  	}
  }
}
</script>

<style>
.finished{
	text-decoration: underline;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
