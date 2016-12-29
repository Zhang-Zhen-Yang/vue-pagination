# vue-pagination

> vue-pagination

## Build Setup


![img](demo.png)

## using

    1.import the single file
    
        import pagination from './Pagination.vue'
    
        components:{pagination},
    
    2.Used in your html
    
        <pagination :options="options" :pagestyle="pagestyle" @goToPage="goToPage">
            <span slot="prevPage">上一页</span>
    		<span slot="nextPage">下一页</span>
        </pagination>
    
    3.Provide the necessary parameters

    
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
      
      
      4.Register event
      
        goToPage (page) {
  		    this.options.currentPage = page
  	    }
  	

    
    


