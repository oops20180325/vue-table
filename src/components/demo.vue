<template>
    <div>
        <div>
          <form id="search">
            Search <input name="query" v-model="filterKey" @keydown="changeCurrentPage">
          </form>
          <table>
            <thead>
              <tr>
                <th v-for="(item,index) in columns "
                v-bind:key="index"
                class="active"
                >
                    {{item | capitalize}}
                    <span class="arrow"
                      :class="sortOrders[item]>0?'asc':'dsc'"
                    ></span>
                </th>
                <th>operation</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(entry,index) in filterData "  v-bind:key="index"            
              >
                <td v-for="(item,index) in columns" v-bind:key='index'>{{entry[item]}}</td> 
                <td>
                  <button  @click="edit(entry)">editEntry</button>
                </td>
              </tr>
            </tbody>
      
          </table>
        </div>
       
    <div>
        <button @click="turnPage(-1)">上一页</button>
        <span>共{{totalPage}}页，当前第{{currentPage}}页</span>
        <button @click="turnPage(1)">下一页</button>
        <span>跳转到第</span><input type="text" v-model="jpage" @keyup.enter='jumpToPage'><span>页</span>
    </div>
    <button @click="log">clicl</button>
    <!-- 具体的项目 -->
    <div id="mask" v-if="editlist">
				<div class="mask">
					<div class="title">
						编辑
						<span @click="editlist=false">
							X
						</span>
					</div>
					<div class="content">
            <!-- 因为数量和信息并不确定所以应该做成动态的 用v-for 做成循环 -->
            <!-- 注意：下面input 唯一的标识是 -->
            <div v-for="(item,index) in editDetail" :key='index'>
                <label >{{index}}</label>
                <input type="text"  
                v-model='editDetail[index]'
                :disabled = 'index=="id"?true:false'
                >
            </div>
						<button @click="update()">更新</button>
						<button @click="editlist=false">取消</button>
					</div>
				</div>
			</div>
    </div>
</template>
<script scoped>
// import _ from 'lodash'; // 排序的 可能会用到
   export default {
    //  props: {
    //   data: Array,
    //   columns: Array,
    // filterKey:String,
    // }
    //  props:['data','columns','filterKey'],
     props:['data','columns'],
     data(){
       var sortOrders = {};//用来存储排序信息
       this.columns.forEach(function(key){
        //  console.log(key)
         sortOrders[key] = 1
       })
      //  console.log(sortOrders)
       return {
         datas:[], // 存传过来的数据的
        //  sortKey:'name',
         sortOrders:sortOrders,
         limit:4,// 每页条数
         jpage:'',// 跳转到某页
         currentPage:1,// 当前显示的页面
         filterArr:[],   // 存放 search 后的 数据
         filterKey:'',  // 储存 search 里的 搜索关键词
         editlist: false, // 编辑层是否可见
         editDetail:{}, // 编辑的信息
         editid:''     // 编辑信息的id
       }
     },
     methods: {
      // sortBy: function (key) {
      //   // console.log(key)
      // this.sortKey = key
      // this.sortOrders[key] = this.sortOrders[key] * -1
      // },
      // 条到某一页
      jumpToPage(){
        var jpage = this.jpage;
        if( jpage<=0||jpage>this.totalPage){
            alert('超过范围啦')
        }else{
            this.currentPage = jpage;
        }
      },
      // 上一页下一页
      turnPage(num){
         console.log(this.totalPage)
         console.log(this.currentPage)
        if(num == 1){
            if(this.currentPage=== this.totalPage){ 
                return
            }else{
                this.currentPage++
                console.log(this.currentPage)
            }
        }else{
            if(this.currentPage===1){
                return
            }else{
                this.currentPage--
            }
        }
      },
      changeCurrentPage(){
        //如果有filterKey 就要从新赋值当前页面成默认以免 换页后搜索 无结果
        this.currentPage = 1;
      },
      // 打开编辑并初始化
      edit(entry){
        this.editlist = true;
        this.editDetail = entry
        console.log(this.editDetail)
        this.editid = entry.id
      },
      //  更新编辑
      update(){
        for( let i = 0 ; i<this.datas.length;i++){
          if(this.datas[i].id ===this.editid){
            this.datas[i] = this.editDetail
            // 关闭editlist层
            this.editlist = false;
          }
        }
      },
      // 
      log(){
        console.log(this.filterData)
      }
    },
    filters: {
      capitalize: function (text) {
        return text[0].toUpperCase() + text.slice(1)
      }
    },
    computed:{
      // 过滤计算后要显示的数据
      filterData(){
        // 排序 还没实现
        // var a = _.orderBy(this.data, 'name',-1)  //排序可能需要

        // console.log(a)
        this.filterArr=this.datas.filter((item)=>{
          // console.log('item' ,item)
            for(var key in item){
              // console.log('key',key)
              var a = item[key]
              var flag =  item[key].toString().toLowerCase().indexOf(this.filterKey.toLowerCase()) !== -1
              // console.log(flag)
              if(flag){
                return true
              }
            }
        })
        // console.log(filterArr)
        var newfilterArr= this.filterArr.slice(this.limit*(this.currentPage-1),this.limit*(this.currentPage));
        // console.log(newfilterArr)
        return newfilterArr
      },
      // 总页数
      totalPage(){
        if(this.filterKey){
          return Math.ceil(this.filterArr.length/this.limit)
        }
        return Math.ceil(this.datas.length/this.limit)
      }
    },
    created(){
      // 把 传过来的数据复制到自己的存数据的地方：props 里的数据不能直接修改
      this.datas = this.data;
      console.log(this.datas)
      
    }
     
   }
  
</script>
<style scoped>
 body {
 font-family: Helvetica Neue, Arial, sans-serif;
 font-size: 14px;
 color: #444;
}
 
table {
 border: 2px solid #4289b9;
 border-radius: 3px;
 background-color: #fff;
 position: relative;
 left: 50%;
 transform: translateX(-50%);
 margin: 10px 0px;
 
}
 
th {
 background-color: #4289b9;
 color: rgba(255,255,255,0.8);
 /* cursor: pointer; */
 -webkit-user-select: none;
 -moz-user-select: none;
 -user-select: none;
}
 
td {
 background-color: #f9f9f9;
}
 
th, td {
 min-width: 120px;
 padding: 10px 20px;
}
 
#search {
 margin-bottom: 10px;
}
.arrow {
 display: inline-block;
 vertical-align: middle;
 width: 0;
 height: 0;
 margin-left: 5px;
 opacity: 0.66;
}
.arrow.asc {
 border-left: 4px solid transparent;
 border-right: 4px solid transparent;
 border-bottom: 4px solid #fff;
}
 
.arrow.dsc {
 border-left: 4px solid transparent;
 border-right: 4px solid transparent;
 border-top: 4px solid #fff;
}
th.active .arrow {
 opacity: 1;
}

/* editlist 层 */
input {
	border: 1px solid #ccc;
	padding: 5px;
	border-radius: 3px;
	margin-right: 15px;
}

button {
	background: #008cd5;
	border: 0;
	padding: 4px 15px;
	border-radius: 3px;
	color: #fff;
}

#mask {
	background: rgba(0, 0, 0, .5);
	width: 100%;
	height: 100%;
	position: fixed;
	z-index: 4;
	top: 0;
	left: 0;
}

.mask {
	width: 300px;
	/* height: 250px; */
	background: rgba(255, 255, 255, 1);
	position: absolute;
	left: 0;
	top: 35%;
	right: 0;
	/* bottom: 0; */
	margin: auto;
	z-index: 47;
	border-radius: 5px;
}

.title {
	padding: 10px;
	border-bottom: 1px solid #eee;
}

.title span {
	float: right;
	cursor: pointer;
}

.content {
	padding: 10px;
}

.content input {
	width: 270px;
	margin-bottom: 15px;
}
</style>
