```js
//ajax----js原生写法
    //1. 创建ajax对象
var xhr
if (window.XMLHttpRequest){
    xhr = new  XMLHttpRequest();
} else { // 兼容IE6, IE5---现已2021年了,作废吧
    xhr = new ActiveXObject("Microsoft.SMLHTTP");
}
    //2. 设置请求地址及方式
//第一个参数用于指定请求方式, 一般用大写
//第二个参数代表请求的URL
//第三个参数表示是否异步发送请求, 默认为true, 表示异步发送
xhr.open("get" , "https://api/xdclass",true)
    //3. 发送请求(没有需要携带的参数时设为null, 有的话作为参数写入, 不填默认null)
shr.send(null)
    //4. 等待浏览器返回结果接受响应
/**
 * on readystate change事件
 *  readyState属性: 请求状态
 *      0   (初始化) 还没有调用open()方法
 *      1   (载入) 已调用send()方法, 正在发送请求
 *      2   (载入完成) send()方法完成, 已收到全部响应内容
 *      3   (解析) 正在解析响应内容
 *      4   (完成) 响应内容解析完成, 可以在客户端调用了
 */
xhr.onreadystatechange = function() {
    if(xhr.readyState == 4) {
        //容错处理, 网络状态码, 200代码成功, 4XX代表客户端错误, 5XX代表服务器错误
        if(xhr.status == 200) {
            alert(xhr.responseText);//将相应转换成文本
        }else{
            alert('出错了, Err:' +xhr.status);
        }
    }
}    

```





vue写法:

使用Axios

util/api.js文件---封装请求

```js
const axios = require('axios')
const baseUrl = 'http://119.45.102.83:3031/'//申请的后端api地址--地址发送变化后修改掉即可
const api = {}

const apiAxios = async (method, url, params) => {
    //项目既定fapp
    let headers = {fapp: 'dmt', 'Content-Type': 'application/json'}
    //读取存储在sessionStorage中的token
    if (sessionStorage.getItem('token')) {
        headers.token = sessionStorage.getItem('token')
    }
    return await new Promise((resolve => {
        axios({
            //如果缓存里有token则所有请求都包含其
            headers: headers,
            method: method,
            url: baseUrl + url,
            //数据内容
            data:
                method === 'POST' ? params : null,
            params:
                method === 'GET' ? params : null
        }).then((res) => {
            console.log(res.data)
            resolve(res.data)
        }).catch(e => {
            console.log(e)
        })
    }))
}

//get请求
api.get = async (url, params,) => {
    return await apiAxios('GET', url, params)
}
//post请求
api.post = async (url, params) => {
    return await apiAxios('POST', url, params)
}

module.exports = api


```

main.js文件---配置请求

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import ViewUI from 'view-design';
import 'view-design/dist/styles/iview.css';
import api from './utils/api'

import Element from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(Element)

Vue.prototype.$api = api
Vue.config.productionTip = false
Vue.use(ViewUI);

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')

```

views/AdminUsers.vue文件---使用请求:

```vue
<template>
    <div>
<!--        用户管理-->
        <!-- 表格内容 -->
        <List>
            <ListItem v-for="item in tableData" :key="item.id" class="item">
                <ListItemMeta :title="item.username" :description="'ip地址:'+item.ip"/>
                <template slot="action">
                    <li>
                        <Button v-on:click="goUrl('users/userInfo/'+item.username)">
                            查看
                        </Button>
                        <Button v-on:click="online(item.username)">
                            {{item.login==0?'封停':'解封'}}
                        </Button>
                        <Button v-on:click="deleteUser(item.username)">
                            删除
                        </Button>
                    </li>
                </template>
            </ListItem>
        </List>
        <!-- 分页器 -->
        <div class="block">
            <el-pagination
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :current-page="currentPage"
                    :page-sizes="[3, 4, 5, 6]"
                    :page-size="pagesize"
                    layout="total, sizes, prev, pager, next, jumper"
                    :total="list.length">
            </el-pagination>
        </div>

    </div>
</template>

<script>


    export default {
        name: "AdminUsers",
        data(){
            return {
                //总的数据
                list:[],
                //刷新页面后每一页有3条数据
                pagesize: 3,
                //刷新页面后当前页为第一页
                currentPage: 1
                //总的数据
            }
        },
        components: {

        },
        //自动更新表格里的数据
        computed: {
            tableData() {
                return this.list.slice(
                    this.pagesize * (this.currentPage - 1),
                    this.pagesize * this.currentPage
                );
            }
        },
        created:function(){
            this.getUsers()
        },
        methods:{
            handleSizeChange(val) {
                //修改每一页的数据量
                this.pagesize = val;
            },
            handleCurrentChange(val) {
                //修改当前页
                this.currentPage = val;
            },
            online(username){  //get请求
                this.$api.get('admin/stopLogin/'+username).then((res)=>{
                    this.$Notice.info({
                        title:'提示',
                        desc:res.message
                    })
                    this.getUsers()
                })
            },
            goUrl(url){
                this.$router.push({path:url})
            },
            deleteUser(username){  //get请求
                this.$api.get('admin/deleteUser/'+username).then((res)=>{
                    this.$Notice.info({
                        title:'提示',
                        desc:res.message
                    })
                    this.getUsers()
                })
            },
            getUsers(){   //get请求
                //获取所有用户
                this.$api.get('admin/getAllUser').then((res)=>{
                    this.list=res.data  //将请求到的数据存在本地list数组中
                })
            }

        }
    }
</script>

<style scoped>

</style>

```



比如上面用到了一个网络请求:  http://119.45.102.83:3031/admin/getAllUser 

请求标头中需额外设置的是:

```json
fapp:dmt
token:***************
```

返回的数据是:

```json
{
    "code": 0,
    "message": "",
    "data": [
        {
            "username": "112",
            "login": 0,
            "ip": "::ffff:112.10.202.112"
        },
        {
            "username": "1122",
            "login": 0,
            "ip": "::ffff:112.10.202.112"
        }
    ]
}
```

