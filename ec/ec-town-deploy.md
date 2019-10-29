# ec-town-deploy
- 统一项目部署
## 前置准备
### 构建小镇官网web端镜像
- 详见[小镇官网web端代码库](https://code.clouderwork.com/cpic/ec-town-web)

### 构建安检服务端镜像
- 详见[安检服务端代码库](https://code.clouderwork.com/cpic/ec-checkin-api)

## 安装部署
1. 拉取[部署代码](https://code.clouderwork.com/cpic/ec-town-deploy)
2. 登录公司阿里云镜像库
3. 执行./pull.sh dev拉取镜像（参数说明：dev研发，prod生产）
4. 修改env-dev（研发）和env-prod（正式）环境配置参数 端口不冲突可使用默认。以研发为例
``` txt
export PROFILE=dev # 部署环境默认dev
export WP_PORT=8899  # wordpress 管理后台端口
export WEB_PORT=8889 # 小镇官网端口
export DB_PORT=3307  # 外网访问数据库端口
export CHECKIN_API_PORT=8898 # 安检服务端端口

export CURRENT_PATH=$(pwd) 
export CONFIG_PATH=${CURRENT_PATH}/conf # 无需修改
export DEPLOY_HOME=${CURRENT_PATH}/deploy # 无需修改，wordpress安装文件位于${DEPLOY_HOME}/web
export COMPOSE_PROJECT_NAME=ec-town # 无需修改
export EC_TOWN_WEB_VERSION=1.0 # 小镇官网镜像版本
export EC_CHECKIN_API_VERSION=1.0 # 安检服务端镜像版本
```
5. 执行./startup.sh 启动服务
6. http://${HOST}:${WP_PORT} （HOST为主机ip或者域名）访问wordpress服务 进行安装wordpress
7. 执行./install_plugin.sh安装wordpress插件（需要[插件代码库](https://code.clouderwork.com/cpic/ec-wp-plugin)权限）
8. 修改wordpress配置

- http://${HOST}:${WP_PORT}/wp-admin/options-permalink.php  选择"文章名"单选项并保存
- http://${HOST}:${WP_PORT}/wp-admin/plugins.php 启用插件
  - Laravel DD for Wordpress
  - REST API TO MiniProgram（开启后需要在菜单上做微信小程序相关配置）
  - WP API SwaggerUI
  - 世界生态大会安检
  - 世界生态大会小程序
  - 世界生态设计小镇
  - 阿里云 OSS （开启后需要在菜单上相应配置）

9. 测试api 返回如下json数据即为成功
``` txt
curl http://${HOST}:${WP_PORT}/wp-json/small_town/v1/hello
{"code":1,"message":"hello","data":""}
```
10. 访问小镇官网 http://${HOST}:${WEB_PORT}

## wordpress更新插件
- 执行./install_plugin.sh （只需首次正确执行安装部署）【无需重启】

## 小镇官网更新（参数说明：dev研发，prod生产）
- 更新并重新构建镜像，详见[小镇官网web端代码库](https://code.clouderwork.com/cpic/ec-town-web)
- 停止 ./cleanup.sh dev
- 执行./pull.sh dev
- 启动 ./startup.sh dev

## 项目启停（参数说明：dev研发，prod生产）
- 启动 ./startup.sh dev
- 停止 ./cleanup.sh dev

## docker环境安装
- 详见[docker安装部署](https://github.com/OracleGao/docker/blob/master/README.md)
