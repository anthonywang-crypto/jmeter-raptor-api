# jmeter-raptor-api

# blazemeter
可以使用blazemeter录制生成jmx脚本（前提需要使用该插件登陆）

<img width="1790" alt="image" src="https://user-images.githubusercontent.com/104351659/205851653-ae0aafe1-b3a0-4b6a-aa73-8eae5ec9bc26.png">

### 场景一
遍历raptor的所有app的实例pod，实现WebSocket接口的login和log

 - jmeter脚本的构造结构
<img width="531" alt="image" src="https://user-images.githubusercontent.com/104351659/206143459-31f0aee5-1acc-4ee5-ac6a-a1754c7f0c82.png">

 - pod的 instance获取+auth+login(websocket) 事务效果图
 可以看到部分错误是connect超时（超过10s）
 <img width="1233" alt="image" src="https://user-images.githubusercontent.com/104351659/206144186-e395f95e-6a31-4d2a-afdf-b3d020bb5d57.png">

 - 聚合结果图 看到失败的概率
 <img width="1775" alt="image" src="https://user-images.githubusercontent.com/104351659/206144558-a6d58f01-6e86-480c-a5a7-5c2d5ebdd6c3.png">

 - 表格结果图 可以查看事务的成功与否
 <img width="1772" alt="image" src="https://user-images.githubusercontent.com/104351659/206145090-658bd7f7-e32a-4fa9-8d3e-fa440958e6bb.png">

 - 结果树 可以查看到事物是否成功，以及事物下面具体的sample请求
 <img width="1079" alt="image" src="https://user-images.githubusercontent.com/104351659/206145602-f1a0d4f6-743c-4928-b2fa-88fb6744d22b.png">
 
 - 遍历过程中token超时的问题
 第250+ 事物登陆时出现token过期
 <img width="1785" alt="image" src="https://user-images.githubusercontent.com/104351659/206156161-6f7811eb-11b1-4577-93b2-b998f2d3db18.png">


 
### 场景二
待补充
