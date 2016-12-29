<template>
  <div id="app">
    	<pagination :options="options" :pagestyle="pagestyle" @goToPage="goToPage">
    		<span slot="prevPage">上一页</span>
    		<span slot="nextPage">下一页</span>
    	</pagination>
  </div>
</template>

<script>
import pagination from './Pagination.vue'
export default {
  name: 'app',
  components:{pagination},
  data () {
    return {
      pagestyle:{
      	cell:{
          'background-color':'#fffffb',
          'color':'#333',
          'padding':'5px 10px',
          'border-radius':'2px',
          'border':'1px solid #e6e6e6',
          'cursor':'pointer',
        },
        disable:{
          'color':'#aaa',
          'background-color':'#efefef',
          'cursor':'default'
        },
        current:{
          'color':'#ffffff',
          'background-color':'#f0ad4e',
          'cursor':'default'
        },
        pageGoTo:{
          'background-color':'#f0ad4e',
          'color':'#ffffff',
          'cursor':'pointer',
          'border-radius':0,
          'border':'1px solid #f0ad4e',
        }
      },
      options:{
      	currentPage:1,//当前页，默认为第1页，不解释
        displayPage:3,//指的是要显示多少个页码，如下图所示，展示了6个，默认显示5个
        displayInfoType:'pageSize',//分页信息的展示，默认展示页数，如果想展示条数，请把该项配置为'itemsCount' 注：如果想展示条数，请通过itemsCount和pageSize来配置分页。
        itemsCount:2000,//一种是直接给出pages,另一种是给出数据的总条数和每页显示的条数
        pageSize:50,        
        showCtrl:true,//是否展示总页数和跳转控制器，默认为false
      }
    }
  },
  methods:{
  	goToPage (page) {
  		this.options.currentPage = page
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

h1, h2 {
  font-weight: normal;
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
