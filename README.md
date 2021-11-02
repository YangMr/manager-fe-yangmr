# OA后台管理系统

[toc]

## 一、项目初始化

### 1.1 在GITHUB上创建项目仓库

### 1.2 将GITHUB上项目仓库克隆到本地

### 1.3 打开命令行进入到本地仓库

### 1.4 使用脚手架在本地仓库创建项目

```
vue create oa-manager
```

### 1.5 进入项目目录并启动项目

```
cd oa-manager && npm run serve
```

###  1.6 返回到本地仓库目录,将项目初始化的代码提交到GIT仓库

```
git add .  && git commit -m "初始化项目"  && git push
```

## 二、插件安装

### 2.1 安装element-ui

```
npm install element-ui --save
```

### 2.2 安装axios

```
npm install axios --save
```

### 2.3 将插件安装的代码提交到GIT仓库

```
git add .  && git commit -m "插件安装"  && git push
```

## 三、目录结构规范

### 3.1 项目目录结构介绍

```
dist							打包之后生成的目录
node_modules			项目的依赖
public						服务器资源目录
src								源代码目录
	api							存放api接口的目录
	assets					静态资源目录
	components			公共组件目录
	config					项目配置目录
	router					路由目录
	store						vuex目录
	utils						工具函数目录
	views						项目页面目录
	App.vue					页面入口组件
	main.js					页面入口js文件
.gitignore				git提交忽略对应内容文件
.env.dev					开发环境变量文件
.env.test					测试环境变量文件
.env.prod					生产环境变量文件
package.json			保存项目依赖文件
vue.config.js			覆盖webpack底层配置文件
package-lock.json		锁定项目依赖版本号配置文件
```

### 3.2 创建对应项目目录结构

### 3.3 vue.confjg.js基础配置

```javascript
module.exports = {
	//配置vue项目打包白屏问题
	publicPath : "./",
	//配置服务器
	devServer : {
		//设置端口号为:8000
		port : 8000,
		//设置主机名
		host : "localhost",
		//设置启动项目时自动打开浏览器
		open : true,
		//关闭https
		https : false
	}
	//关闭eslint
	lintOnSave : false
}
```

### 3.4 将目录结构规范的代码提交到GIT仓库

```
git add .  && git commit -m "插件安装"  && git push
```

## 四、环境配置

### 4.1 在config/index.js文件内对环境进行封装

```javascript
/*config/index.js*/
/**
*	环境配置封装
**/
const env = import.meta.env.MODE || "prod";

const EnvConfig = {

	dev : {
		baseApi : "/",
		mockApi : "https://www.fastmock.site/mock/76ef040b1066810a6a9e8b7cf636e63d/oa",
	},
	
	test : {
		baseApi : "//test.futurefe.com/api",
		mockApi : "https://www.fastmock.site/mock/76ef040b1066810a6a9e8b7cf636e63d/oa",
	},
	
	prod : {
		baseApi : "//futurefe.com/api",
		mockApi : "https://www.fastmock.site/mock/76ef040b1066810a6a9e8b7cf636e63d/oa",
	}

}

export default {
	env,
	mock : true,
	...EnvConfig[env]
}
```

### 4.2 将环境配置的代码提交到GIT仓库

```
git add .  && git commit -m "环境配置"  && git push
```

## 五、axios二次封装

### 5.1 在utils文件夹内创建request.js文件

### 5.2 在request.js文件内对axios进行二次封装

```javascript
/**
*	axios二次封装
*/

//1. 引入axios
import axios from "axios"

//2. 引入环境变量配置文件
import config from "../config/index.js"

//6. 引入element-ui的message组件
import { Message } from 'element-ui';

//7. 自定义错误状态码提示信息
const TOKEN_INVALID = "TOKEN认证失败,请重新登录"

//8. 导入路由对象
import router from "../router"

//9. 定义网络错误提示信息
const NETWORK_ERROR = "网络请求异常,请稍后重试" 

//3. 创建axios实例对象,添加全局请求配置
const service = axios.create({
  //设置请求的公共接口地址
  baseURL : config.baseApi
  //设置请求超时时间
  timeout : 8000
})

//4. 创建请求拦截器
service.interceptors.request.use((req)=>{
  const headers = req.headers;
  if(!headers.Authorization) headers.Authorization = "要发送的token"
  return req;
})

//5. 创建响应拦截器
service.interceptors.response.use((res)=>{
  const {code,data,msg} = res.data;
  if(code === 200){
     return data;
  }else if(code === 40001){
    	Message.error(TOKEN_INVALID);
    	setTimeout(()=>{
        router.push("/login");
      },1500)
    	return Promise.reject(TOKEN_INVALID);
  }else{
    	Message.error(msg || NETWORK_ERROR)
    	return Promise.reject(msg || NETWORK_ERROR)
  }
})

//10. 定义请求核心函数
function request(options){
  options.method = options.method || "get";
  
  if(options.method.toLowerCase() == "get"){
     options.params = options.data;
  }
  
  if(config.env === "prod"){
     service.defaults.baseURL = config.baseApi
  }else{
     service.defaults.baseURL = config.mock ? config.mockApi : config.baseApi
  }
  
  return service(options)
}

//12. 处理使用this.request.get/post/put/delete/patch这几种请求方法
/*
	this.$request.get("/login",{name : "jack"},{mock:true, loading : true}).then(res=>{
		console.log(res)
	})
*/

["get","post","put","delete","patch"].forEach((item)=>{
  request[item] = (url,data,options)=>{
    return request({
      url,
      data,
      method : item,
      ...options
    })
  }
})

//11. 导出请求方法
export default request;

```

### 5.3 将axios二次封装的代码提交到GIT仓库

```
git add .  && git commit -m "axios二次封装"  && git push
```

## 六、storage二次封装

### 6.1 在utils文件夹内创建storage.js文件

### 6.2 在config/index.js文件内创建存储的命名空间

```javascript
const env = import.meta.env.MODE || "prod";

const EnvConfig = {

	dev : {
		baseApi : "/",
		mockApi : "https://www.fastmock.site/mock/76ef040b1066810a6a9e8b7cf636e63d/oa",
	},
	
	test : {
		baseApi : "//test.futurefe.com/api",
		mockApi : "https://www.fastmock.site/mock/76ef040b1066810a6a9e8b7cf636e63d/oa",
	},
	
	prod : {
		baseApi : "//futurefe.com/api",
		mockApi : "https://www.fastmock.site/mock/76ef040b1066810a6a9e8b7cf636e63d/oa",
	}

}

export default {
	env,
	mock : true,
	//定义本地存储的命名空间
	namespace : "manage",
	...EnvConfig[env]
}
```

### 6.3 在storage.js文件内对本地存储进行封装

```javascript
/*utils/storage*/

/***
* Storage二次封装
*/
//1. 在config/index.js文件内创建存储的命名空间 "namespace" : "manage",在storage.js文件内引入config/index.js

import config from "../config/index.js"



//2. 封装storage
export default {
  setItem(key,val){
    let storage = this.getStorage();
    storage[key] = val;
    window.localstorage.setItem(config.namespace,JSON.stringify(storage));
  },
  getItem(key){
    return this.getStorage()[key];
  },
  getStorage(){
    return JSON.parse(window.localstorage.getItem(config.namespace) || "{}");
  },
  clearItem(key){
    let storage = this.getStorage();
    delete storage[key];
    window.localstorage.setItem(config.namespace,JSON.stringify(storage));
  },
  clearAll(){
    window.localstorage.clear()
  }
}
```

### 6.4 将storage二次封装的代码提交到GIT仓库

```
git add .  && git commit -m "storage二次封装"  && git push
```



## 七、用户登录布局开发

## 八、用户登录交互开发

## 九、用户登录后台开发