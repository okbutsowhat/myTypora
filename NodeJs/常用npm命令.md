# 常用npm命令

## 一、镜像源相关

### 1. 查看镜像源设置

```shell
npm config get registry 
```



### 2. 设置镜像源

> 海康镜像源：
>
> ```shell
> npm config set registry http://af.hikvision.com.cn/artifactory/api/npm/npm-down/
> ```
>
> 淘宝镜像源：
>
> ```shell
> npm config set registry https://registry.npm.taobao.org
> ```

### 3.  安装cnpm

```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```



