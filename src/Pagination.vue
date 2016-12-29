<template>
  <div class="pagination">    
    <div class="prevPage page" 
        @click="prevPage" 
        :class="{disable:options.currentPage==1}" 
        :style="options.currentPage==1?computedcellStyle.disable:computedcellStyle.cell">
        <slot name="prevPage"></slot>
    </div>
    <div class="page" 
        v-for="($val,$key) in leftPages" 
        :class="{currentPage:options.currentPage == $val}" 
        @click="selectPage($val)" 
        :style="options.currentPage == $val?computedcellStyle.current:computedcellStyle.cell">
        {{ $val }}
    </div>
    <span v-if="centerPages.length>0">...</span>
    <div class="page" v-for="($val,$key) in centerPages" 
        :class="{currentPage:options.currentPage == $val}" 
         @click="selectPage($val)"
         :style="options.currentPage == $val?computedcellStyle.current:computedcellStyle.cell">
        {{ $val }}
    </div>
    <span v-if="rightPages&&rightPages.length>0">...</span>
    <div class="page" v-for="($val,$key) in rightPages" :class="{currentPage:options.currentPage == $val}" @click="selectPage($val)" :style="computedcellStyle.cell">
        {{ $val }}
    </div>
    <div class="nextPage page" 
        @click="nextPage" 
        :style="options.currentPage==options.pages?computedcellStyle.disable:computedcellStyle.cell">
        <slot name="nextPage"></slot>
    </div>
   <template v-if="options.showCtrl">
    <template v-if="options.displayInfoType=='pageSize'">共 {{ options.pages }} 页&nbsp;</template>
    <template v-if="options.displayInfoType=='itemsCount'">共 {{ options.itemsCount }} 项&nbsp;</template>
    到
    
    <div class="go-to-page-wrap" 
        :style="{'border-color':computedcellStyle.pageGoTo['background-color']||computedcellStyle.pageGoTo.backgroundColor}">
        <input type="number" min="0" class="go-to-page" @focus="inputFofus=true" @blur="inputFofus=false" v-model="goToPage"  />
        <div class="go-to-page-comfirm" :class="{'focus-input':inputFofus}" @click="goPageComfirm" :style="computedcellStyle.pageGoTo">
            确定
        </div>
    </div>
    页
    </template>
    
  </div>
</template>

<script>

export default {
  name: 'pagination',
  props:{
      pagestyle:{
          type:Object,
          default (){
              return {
                  
              }
          }
      },
      options:{
          type:Object,
          default () {
            return {
                currentPage:1,//当前页，默认为第1页，不解释
                displayPage:3,//指的是要显示多少个页码，如下图所示，展示了6个，默认显示5个
                displayInfoType:'pageSize',//分页信息的展示，默认展示页数，如果想展示条数，请把该项配置为'itemsCount' 注：如果想展示条数，请通过itemsCount和pageSize来配置分页。
                
                
                itemsCount:1000,//一种是直接给出pages,另一种是给出数据的总条数和每页显示的条数
                pageSize:50,
                pages:1000,
                showCtrl:true,//是否展示总页数和跳转控制器，默认为false
            }
          }
      }
  },
  data () {
    return {
        defaultStyle:{
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
                'cursor':'default',
            }
        },
        inputFofus:false,
        goToPage:''
      
    }
  },//end data
  computed:{
      leftPages () {
          let arr=[];
          if(this.options.pages<=this.options.displayPage+1){
              for(let i=1;i<=this.options.pages;i++){
                  arr.push(i);
              }
          }else if(this.options.currentPage+1 >= this.options.displayPage){
              arr.push(1)
          }else {
              for(let i=1;i<=this.options.displayPage-1;i++){
                  arr.push(i);
              }
          }
          return arr;
         
      },
      centerPages () {
        if(this.options.pages<=this.options.displayPage+1) return [];
        let count = Math.ceil((this.options.displayPage -2)/2)-1,arr=[];
        if(this.options.currentPage+1 >= this.options.displayPage){
             for(let i=(this.options.currentPage-count),j=0;j<(this.options.displayPage - 2);i++,j++){
                arr.push(i);
             }
        }
        if(arr[arr.length-1]>=this.options.pages){
            arr=[];
            for(let i=this.options.pages,j =this.options.pages - this.options.displayPage+1;j<this.options.pages;i--,j++){
                arr.unshift(i);
             }
        }
         return arr;
      },
      rightPages () {
        if(this.options.pages<=this.options.displayPage+1) return [];
         if((this.centerPages[this.centerPages.length-1]+1<=this.options.pages)||this.centerPages.length==0){
             return [this.options.pages]
         }
      },
      computedcellStyle(){
          let cell = Object.assign({},this.defaultStyle.cell,this.pagestyle.cell||{}),
            current = Object.assign({},cell,this.pagestyle.current||this.defaultStyle.current),
            disable = Object.assign({},cell,this.pagestyle.disable||this.defaultStyle.disable),
            pageGoTo = Object.assign({},cell,this.pagestyle.pageGoTo||this.defaultStyle.pageGoTo);
          return {
              cell,
              current,
              disable,
              pageGoTo
          }
      }

  },//end computed
  methods:{
      selectPage (page) {
      	this.$emit('goToPage',page);
      },
      prevPage () {
          if (this.options.currentPage!=1)
          this.$emit('goToPage',this.options.currentPage - 1);
      },
      nextPage () {
          if (this.options.currentPage!=this.options.pages)
          this.$emit('goToPage',this.options.currentPage + 1);
          
      },
      goPageComfirm () {
          this.$emit('goToPage',this.goToPage);
    }
  },//end methods
  created () {
      this.options.pages= Math.ceil(this.options.itemsCount / this.options.pageSize) || this.options.pages || 1;
      this.options.displayPage = this.options.displayPage < 6 ? 5: this.options.displayPage
  }
}
</script>

<style scoped>
    .pagination {
        text-align:center;
        font-size:14px;
        -webkit-user-select:none;
        user-select:none;
    }
    .pagination>div{
        display:inline-block;
        margin: 0 3px 
    }
    .go-to-page-wrap{
       border:1px solid;
    }
    .go-to-page{
        outline:none;
        border:none;
        padding:4px 5px;
        width:70px;
        height:100%;
    }
    .go-to-page-comfirm{
        display:inline-block;
        padding:4px 10px;
        margin-left:-45px;
        cursor:pointer;
        transition:all 0.2s linear;
    }
    .focus-input{
        margin-left:-25px;
    }
</style>