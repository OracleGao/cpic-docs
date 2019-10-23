# ec-town-web
- 小镇官网
## docker镜像构建
1. 执行./build.sh 
2. 登录公司阿里云docker镜像库
3. 执行./push.sh

## 部署
- 详见[部署](https://code.clouderwork.com/cpic/ec-town-deploy)

## docker环境安装
- 执行下面指令（仅支持ubuntu和centos，请确保操作系统内核版本符合docker官方要求）
``` txt
curl -L github.com/OracleGao/docker/raw/master/docker-install.sh | bash
curl -L github.com/OracleGao/docker/raw/master/docker-compose-install.sh | bash
```
