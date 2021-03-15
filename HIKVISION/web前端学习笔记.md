# 第一期

### 目标: 

### 1.  使用公司脚手架搭建前端工程，熟悉项目模块；

### 2.  搭配HUI开发项目；

### 过程记录:

#### 使用公司前端脚手架Vessel搭建前端工程:

1. vue环境安装:

    ```sh
    npm install -g @vue/cli
    ```

2. 设置公司的npm仓库源

3. 分公司由于网络问题需更改系统目录下的.vuerc文件配置,再通过以下命令创建项目:

    ```sh
    vue create your-project-name
    ```

4. 进入项目根目录并启动项目:

    ```sh
    cd my-vessel-pj
    npm run serve
    ```

    ![微信截图_20200709103841](C:\Users\liuxiatian\Pictures\微信截图_20200709103841.png)

#### 熟悉Vessel项目模块 —— 菜单：

1. 配置导航栏配置文件，新增一个菜单项

    ```json
    export default [
      {
        title: 'hello.welcome',
        router: '/hello',
        menuCode: '001',
        icon: 'h-icon-menu'
      },
      {
        title: '新增页面',
        router: '/test',
        menuCode: '002',
        icon: 'h-icon-menu'
      }
    ]
    ```

2. 配置路由配置文件

    ```json
    {
      name: 'test',
      path: '/test/',
      menuCode: '002',
      component: 'Test'
    }
    ```

3. 在pages目录下新建所需要加入的页面组件并命名为Test.vue

4. 配置action.js下的对应字段

    ```json
    breadcrumb: {
      '001': ['欢迎'],
      '002': ['测试页面']
    },
    code: [
      `${process.env.VUE_APP_CONTEXT}_001`,
      `${process.env.VUE_APP_CONTEXT}_002`
    ]
    ```

![微信截图_20200709103941](C:\Users\liuxiatian\Pictures\微信截图_20200709103941.png)



#### 使用dolphin-learning插件实现数据联调：

1. 安装对应的环境，git、nodejs等

2. 进入项目目录添加插件：

   ```sh
   vue add dolphin-learning
   ```

3. 默认为应英文，下载多语言包：

   ```sh
   npm run i18n:download
   ```

4. 