# ec-town-deploy

## 前值准备
### 构建小镇官网web端镜像
- 详见[小镇官网web端代码库](https://code.clouderwork.com/cpic/ec-town-web)

## 安装部署
1. 拉取项目代码
2. 登录公司阿里云镜像库
3. 执行./pull.sh 拉取镜像
4. 修改env环境配置参数 端口不冲突可使用默认

```
export PROFILE=dev # 部署环境默认dev
export WP_PORT=8899  # wordpress 管理后台端口
export DB_PORT=3307  # 外网访问数据库端口
export CURRENT_PATH=$(pwd) 
export CONFIG_PATH=${CURRENT_PATH}/conf # 无需修改
export DEPLOY_HOME=${CURRENT_PATH}/deploy # 无需修改，wordpress安装文件位于${DEPLOY_HOME}/web

export COMPOSE_PROJECT_NAME=ec-town # 无需修改

export EC_TOWN_WEB_VERSION=1.0 # 小镇官网镜像版本

```
5. 执行./startup.sh 启动服务
6. http://${HOST}:${WP_PORT} 访问wordpress服务 进行安装wordpress
7. 执行./install_plugin.sh安装wordpress插件（需要[插件代码库](https://code.clouderwork.com/cpic/ec-wp-plugin)权限）
8. 修改wordpress配置

  1. http://${HOST}:${WP_PORT}/wp-admin/options-permalink.php  选择"文章名"单选项并保存
  2. http://${HOST}:${WP_PORT}/wp-admin/plugins.php 启用插件
```
Laravel DD for Wordpress
REST API TO MiniProgram
WP API SwaggerUI
世界生态大会安检
世界生态大会小程序
世界生态设计小镇
```

9. 测试api 返回如下json数据即为成功

```
curl http://${HOST}:${WP_PORT}/wp-json/small_town/v1/hello
{"code":1,"message":"hello","data":""}
```
## wordpress更新插件
- 正确执行安装部署后 执行./install_plugin.sh 即可

## docker环境安装
- 执行下面指令（仅支持ubuntu和centos，请确保操作系统内核版本符合docker官方要求）

```
curl -L github.com/OracleGao/docker/raw/master/docker-install.sh | bash
curl -L github.com/OracleGao/docker/raw/master/docker-compose-install.sh | bash
```
