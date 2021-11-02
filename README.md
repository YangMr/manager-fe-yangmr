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

#### 4.1 在config/index.js文件内对环境进行封装

```javascript
/*config/index.js*/

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
git add .  && git commit -m "插件安装"  && git push
```

