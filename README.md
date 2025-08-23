## MOWEN - 玄牝之门，是谓天地根

> 项目基于[POETIZE](https://gitee.com/littledokey/poetize)：最美博客！作诗，有诗意地描写。"三年沉淀，一生所爱" —— 一个程序员的全栈浪漫。



### 技术栈

| 模块     | 技术栈         | 组件                                |
| -------- | -------------- | ----------------------------------- |
| 后端     | SpringBoot 2.7 | Nginx + MySQL + Mybatis-Plus + T-io |
| 博客     | Vue2           | Element UI                          |
| 聊天室   | Vue3           | Element-Plus UI + Naive UI          |
| 移动适配 | CSS            | 完美自适应                          |

网站分两个模块：

- 博客系统：具有文章，表白墙，图片墙，收藏夹，音乐播放，视频播放，留言，友链，后台管理等功能。
- 聊天室系统：具有朋友圈，好友，群等功能。

本网站采用前后端分离进行实现，两个前端项目通过Nginx代理，后端使用Java，数据存储使用Mysql。

部署网站需要安装Nginx、Java、MySQL，然后打包前后端项目并部署。

文件服务可以使用七牛云，也可以使用服务器。默认使用服务器。

Vue3（IM 聊天室系统）是非必须的。如果部署，则需要依赖博客，然后从博客的“联系我”进入，因为登录模块在博客。

### 本地启动

- 前端（博客系统、聊天室系统）：

  ``` bash
  # 安装依赖
  npm i
  
  # 运行
  npm start
  npm run dev
  npm run serve
  ```

- 后端（SpringBoot）：

  - 导入SQL文件`poetry.sql`到数据库

  - 配置数据库连接（application.yml里面的datasource信息）

  - 启动（PoetryApplication）

- 登录**网站后台（[http://localhost/admin）](https://gitee.com/link?target=http%3A%2F%2Flocalhost%2Fadmin%EF%BC%89) - 配置管理**可以配置网站信息。请注意部分配置信息修改后需要重启Java，比如邮箱号和邮箱授权码。

- 本地启动项目后，文件无法访问，因为没有通过Nginx代理，浏览器无法直接访问文件。服务器部署后，文件通过Nginx即可访问。

- 默认管理员账号`ZS`密码`aaa`