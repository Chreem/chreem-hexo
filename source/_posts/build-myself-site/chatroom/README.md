# 仿gitter的Chatroom

所谓仿仅为功能仿而非UI仿 ~~(审美残疾...)~~

## 后端API

各类API均需在进行登录请求拿到token后将token加入请求头

|API Name|Method|Link|参数说明|返回值|
|---|:---:|---|---|---|
|User|GET|`http://xxx/user/{UserID}`|{UserID}: 用户id|用户信息对象:`{'name':'xxx'}`|
||GET|`http://xxx/user{?UserID=[1,2,3]}`|{UserID=list}|用户信息的数组:`[{'name':'user1'},{'name':'user2'}]`|
||POST|`http://xxx/user/`|user: 将创建用户信息的对象|创建成功: 包含UserID的用户信息/创建失败:-1|
||DELETE|`http://xxx/user/`|userid: 将删除用户的ID|删除成功: 0/删除失败: -1|
|User&Chatroom|GET|`http://xxx/user/{UserID}/chatroom`||该用户加入的所有chatroom信息及其权限|
||GET|`http://xxx/user/{UserID}/chatroom/{ChatroomID}`||效果等同于`http://xxx/chatroom/{ChatroomID}`, 区别在于user拥有相应权限则返回所有信息否则仅为简略信息|
|Chatroom|GET|`http://xxx/chatroom/{ChatroomID}`||chatroom简略信息|
||GET|`http://xxx/chatroom{?ChatroomID=[1,2,3]}`|||
|---|---|---|---|---|
|Actions|POST|`http://xxx/actions/login`|account,password|验证成功: token/验证失败: -1|
||POST|`http://xxx/actions/logout`|userid|注销成功: 0/失败: -1|
||POST|`http://xxx/actions/state`|userid|登录状态: 0/登出: -1|
