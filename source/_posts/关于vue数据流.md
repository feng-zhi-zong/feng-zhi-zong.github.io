---
title: 关于vue数据流
date: 2018-08-03 15:15:31
tags:
	- vue
	- 数据
mp3: http://music.163.com/song/media/outer/url?id=16959121&userid=1918806314.mp3
cover: /img/tigase-pre-research-1.jpeg

---

## vuex数据流

说实话，看了官方文档我并不清楚vuex到底是个什么东西，通过示例，我知道如何使用后，我才慢慢理解一点。
我的理解就是：
为什么会存在组件不能通信？简单粗暴的回答就是，变量的作用域导致的，vue的每一个组件都有自己的生命周期，都是独立的个体。vuex就是在根组件创建一个叫状态的变量，让不同的组件共享这个变量，并且定义了各种各样的方法和规范，也就是action，mutation等等，去统一触发这个变量变量也就是state。



```
// store / index.js     从'vue' 导入 Vue 从'vuex'
导入 Vue  Vue.use（Vuex）
const store = new Vuex.Store（{   
	state（）{ 
		return {       
			monitorInfo：{         
				name：``，        
				description：''       
			}     
		}   
	}}，  
操作：{     updateMonitorInfo：（{commit}，info）=> {       commit（'UPDATE_MONITOR_INFO '，info）    }   }，  突变：{     UPDATE_MONITOR_INFO：（state，info）=> { 		window .localStorage.setItem（' monitorInfo '，	JSON.stringify（info））              
	state.monitorInfo = info     
}   }，
获取方法  ：{     monitorInfo：（state）=> { if（！state.monitorInfo.name）
	{         state.monitorInfo = JSON .parse（window .localStorage.getItem（' monitorInfo '））      } 
	返回 state.monitorInfo     }   }，  
模块：{   } }）导出默认存储             
```



```js
<！-列出兄弟组件的发送方-> 
 < 模板 >
 </ template >
< script >  从 'vuex' 导入 {mapActions} 导出默认 {     数据（）{ 返回 {       }     }，    
计算    方法：{     }，
方法：{         ... mapActions（[ 'updateMonitorInfo' ]），      
goNextPage（row，routerName）{ 让 monitorInfo = {           名称：row.name，          
    serviceUrl：row.serviceUrl，          
描述：row.info.description         } 此 .updateMonitorInfo（monitorInfo）      
     }，    }，
 已安装（）{     }   }                         
</ 脚本 >

<！-monitor.vue兄弟组件接收方-> 
< 模板 > 
< span > {{monitorInfo.name}} </ span >
< span > {{monitorInfo.description}} </ span > 
</ template >   
< 脚本 >  从 'vuex' 导入 {mapGetters} 导出默认 {     数据（）{ 返回 {       }     }，    
计算方式：{       ... mapGetters（[ ' monitorInfo ' ]）    }，
方法：{        }，
已安装（）{     }   }         
</ 脚本 >
```

