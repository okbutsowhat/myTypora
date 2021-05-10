# videoPlugin Vue封装

## 组件封装

```vue
<template>
   <div :id="id" class="playWnd"></div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'VideoPreview',
  props: {
    cameraIndexCode: { // 监控点编号
      type: String,
      default: ''
    },
    videoConfig: { // 插件初始化配置
      type: Object,
      default: () => ({
        // 按需修改下方videoConfigFinal需修改处
      })
    },
    playConfig: { // 视频播放配置
      type: Object,
      default: () => ({
        streamMode: 0, // 主子码流标识：0-主码流，1-子码流
        transMode: 1, // 传输协议：0-UDP，1-TCP
        gpuMode: 0, // 是否启用GPU硬解，0-不启用，1-启用
        wndId: -1, // 播放窗口序号（在2x2以上布局下可指定播放窗口）
        ezvizDirect: 0, // 萤石直连
        recordLocation: 1, // 录像存储位置 0-中心存储 1-设备存储
        // 默认调取当前时间往前推一天的回放
        // 时间戳对照表: 1min = 60; 1hour = 3600; 1day = 86400; 1week = 604800; 1month 2629743; 1year = 31556736
        endTimeStamp: Math.round(new Date().getTime() / 1000), // 时间戳, 获取当前时间戳方法: Math.round(new Date().getTime() / 1000)
        playTimeStamp: Math.round(new Date().getTime() / 1000) - 86400,
        startTimeStamp: Math.round(new Date().getTime() / 1000) - 86400
      })
    }
  },
  data () {
    return {
      id: this.getUUID(), // 容器id, 使用随机UUID避免重复
      oWebControl: null,
      initCount: 0,
      pubKey: '',
      screenWidth: document.body.clientWidth,
      playWindowSize: [],
      videoConfigFinal: {
        ip: '', // 综合安防管理平台IP地址
        appkey: '', // 综合安防管理平台提供的appkey
        secret: '', // 综合安防管理平台提供的secret
        port: 443, // 综合安防管理平台端口，若启用HTTPS协议
        enableHTTPS: 1, // 是否启用HTTPS协议与综合安防管理平台交互
        playMode: 0, // 初始播放模式：0-预览，1-回放
        snapDir: 'D:\\SnapDir', // 抓图存储路径
        videoDir: 'D:\\VideoDir', // 紧急录像或录像剪辑存储路径
        layout: '1x1', // playMode指定模式的布局
        encryptedFields: 'secret', // 加密字段，默认加密领域为secret
        showToolbar: 1, // 是否显示工具栏，0-不显示，非0-显示
        showSmart: 0, // 是否显示智能信息（如配置移动侦测后画面上的线框），0-不显示，非0-显示
        buttonIDs: '0,16,256,257,258,259,260,512,513,514,515,516,517,768,769', // 自定义工具条按钮
        ...this.videoConfig
      }
    }
  },
  computed: {
  },
  watch: {
    // 监听resize事件，使插件窗口尺寸跟随DIV窗口变化
    screenWidth (val) {
      this.screenWidth = val
      if (this.oWebControl !== null) {
        this.setNewPlayWindowSize()
        this.oWebControl.JS_Resize(...this.playWindowSize)
        this.setWndCover()
      }
    },
    $route: {
      handler: function (val) {
        if (val.fullPath === '/') {
          if (this.oWebControl !== null) {
            this.oWebControl.JS_ShowWnd()
          }
        } else {
          if (this.oWebControl !== null) {
            this.oWebControl.JS_HideWnd() // 先让窗口隐藏，规避可能的插件窗口滞后于浏览器消失问题
          }
        }
      },
      // 深度观察监听
      deep: true
    },
    oWebControl () {
      if (this.oWebControl === null) {
        this.initPlugin()
      }
    },
    cameraIndexCode (val) {
      this.playVideo(val)
    }
  },
  mounted () {
    window.onselectstart = function () { return false }
    this.initPlugin()
    const that = this
    window.onresize = () => { // 监听resize事件
      this.setNewPlayWindowSize()
      this.oWebControl.JS_Resize(...this.playWindowSize)
      return (() => {
        window.screenWidth = document.body.clientWidth
        that.screenWidth = window.screenWidth
      })()
    }
    document.addEventListener('scroll', this.handleScroll, true) // 监听滚动条scroll事件，使插件窗口跟随浏览器滚动而移动
  },
  beforeDestroy () {
    if (this.oWebControl !== null) {
      this.oWebControl.JS_HideWnd() // 先让窗口隐藏，规避可能的插件窗口滞后于浏览器消失问题
      this.oWebControl.JS_Disconnect().then(() => {
        // 断开与插件服务连接成功
      }, () => {
        // 断开与插件服务连接失败
      })
    }
    window.removeEventListener('scroll', this.handleScroll, true) // 清除事件监听
  },
  methods: {
    getUUID () {
      const str = 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'
      return str.replace(/[xy]/g, item => {
        const r = Math.random() * 0x10 | 0
        const v = item === 'x' ? r : (r & 0x3 | 0x8)
        return v.toString(0x10)
      })
    },
    playVideo (cameraIndexCode) {
      console.log(cameraIndexCode)
      if (!cameraIndexCode.length) return
      const playMode = this.videoConfigFinal.playMode // 播放模式 0-预览 1-回放
      if (playMode === 0) {
        this.oWebControl.JS_RequestInterface({
          funcName: 'startPreview',
          argument: JSON.stringify({
            cameraIndexCode: cameraIndexCode, // 监控点编号
            ...this.playConfig
          })
        })
      } else if (playMode === 1) {
        this.oWebControl.JS_RequestInterface({
          funcName: 'startPlayback',
          argument: JSON.stringify({
            cameraIndexCode: cameraIndexCode, // 监控点编号
            ...this.playConfig
          })
        })
      }
    },
    /**
     * @desc: 创建播放实例
     * @param:
     */
    initPlugin () {
      this.oWebControl = new WebControl({
        szPluginContainer: this.id, // 指定容器id
        iServicePortStart: 15900, // 指定起止端口号，建议使用该值
        iServicePortEnd: 15909,
        szClassId: '23BF3B0A-2C56-4D97-9C03-0CB103AA8F11', // 用于IE10使用ActiveX的clsid
        cbConnectSuccess: () => {
          this.oWebControl.JS_StartService('window', { // WebControl实例创建成功后需要启动服务
            dllPath: './VideoPluginConnect.dll' // 值"./VideoPluginConnect.dll"写死
          }).then((res) => { // 启动插件服务成功
            this.oWebControl.JS_SetWindowControlCallback({ // 设置消息回调
              cbIntegrationCallBack: function (oData) { // oData 是封装的视频 web 插件回调消息的消息体
                console.log(oData) // 打印消息体至控制台
              }
            })
            this.setNewPlayWindowSize()
            this.oWebControl.JS_CreateWnd(this.id, ...this.playWindowSize).then(() => { // JS_CreateWnd创建视频播放窗口，宽高可设定
              this.oWebControl.JS_RequestInterface({
                funcName: 'getRSAPubKey',
                argument: JSON.stringify({
                  keyLength: 1024
                })
              }).then((oData) => {
                if (oData.responseMsg.data) {
                  this.pubKey = oData.responseMsg.data
                  this.init() // 创建播放实例成功后初始化
                }
              })
            })
          }, (err) => { // 启动插件服务失败
            console.log('启动插件服务失败++++', err)
          })
        },
        cbConnectError: () => { // 创建WebControl实例失败
          this.oWebControl = null
          console.log('插件未启动，正在尝试启动，请稍候...')
          WebControl.JS_WakeUp('VideoWebPlugin://') // 程序未启动时执行error函数，采用wakeup来启动程序
          this.initCount++
          if (this.initCount < 3) {
            setTimeout(() => {
              this.initPlugin()
            }, 3000)
          } else {
            console.log('插件启动失败，请检查插件是否安装！')
          }
        },
        cbConnectClose: () => {
          this.oWebControl = null
        }
      })
    },
    /**
     * @desc: 初始化
     * @param:
     */
    init () {
      this.oWebControl.JS_RequestInterface({
        funcName: 'init',
        argument: JSON.stringify({
          ...this.videoConfigFinal,
          secret: this.setEncrypt(this.videoConfigFinal.secret),
        })
      }).then((oData) => {
        this.setNewPlayWindowSize()
        this.oWebControl.JS_Resize(...this.playWindowSize) // 初始化后resize一次，规避firefox下首次显示窗口后插件窗口未与DIV窗口重合问题
        this.playVideo(this.cameraIndexCode)
      })
    },
    handleScroll () {
      if (this.oWebControl !== null) {
        this.setNewPlayWindowSize()
        this.oWebControl.JS_Resize(...this.playWindowSize)
        this.setWndCover()
      }
    },
    resizeWin () {
      this.setNewPlayWindowSize()
      this.oWebControl.JS_Resize(...this.playWindowSize)
    },
    /**
     * @desc: 获取视频dom宽高
     */
    setNewPlayWindowSize () {
      const _dom = document.getElementById(this.id)
      const _w = parseInt(window.getComputedStyle(_dom).width.replace('px', ''))
      const _h = parseInt(window.getComputedStyle(_dom).height.replace('px', ''))
      this.playWindowSize = [_w, _h]
      return [_w, _h]
    },
    /**
     * @desc: 设置窗口裁剪，当因滚动条滚动导致窗口需要被遮住的情况下需要JS_CuttingPartWindow部分窗口
     */
    setWndCover () {
      const wh = this.setNewPlayWindowSize()
      const iWidth = window.innerWidth
      const iHeight = window.innerHeight
      const oDivRect = window.document.getElementById(this.id).getBoundingClientRect()
      let iCoverLeft = (oDivRect.left < 0) ? Math.abs(oDivRect.left) : 0
      let iCoverTop = (oDivRect.top < 0) ? Math.abs(oDivRect.top) : 0
      let iCoverRight = (oDivRect.right - iWidth > 0) ? Math.round(oDivRect.right - iWidth) : 0
      let iCoverBottom = (oDivRect.bottom - iHeight > 0) ? Math.round(oDivRect.bottom - iHeight) : 0
      iCoverLeft = (iCoverLeft > wh[0]) ? wh[0] : iCoverLeft
      iCoverTop = (iCoverTop > wh[1]) ? wh[1] : iCoverTop
      iCoverRight = (iCoverRight > wh[0]) ? wh[0] : iCoverRight
      iCoverBottom = (iCoverBottom > wh[1]) ? wh[1] : iCoverBottom
      this.oWebControl.JS_RepairPartWindow(0, 0, wh[0] + 1, wh[1]) // 多1个像素点防止还原后边界缺失一个像素条
      if (iCoverLeft !== 0) {
        this.oWebControl.JS_CuttingPartWindow(0, 0, iCoverLeft, wh[1])
      }
      if (iCoverTop !== 0) {
        this.oWebControl.JS_CuttingPartWindow(0, 0, wh[0] + 1, iCoverTop) // 多剪掉一个像素条，防止出现剪掉一部分窗口后出现一个像素条
      }
      if (iCoverRight !== 0) {
        this.oWebControl.JS_CuttingPartWindow(wh[0] - iCoverRight, 0, iCoverRight, wh[1])
      }
      if (iCoverBottom !== 0) {
        this.oWebControl.JS_CuttingPartWindow(0, wh[1] - iCoverBottom, wh[0], iCoverBottom)
      }
    },
    /**
     * @desc: RSA加密
     * @param: value 综合安防管理平台提供的secret
     */
    setEncrypt (value) {
      const encrypt = new JSEncrypt()
      encrypt.setPublicKey(this.pubKey)
      return encrypt.encrypt(value)
    }
  }
}
</script>

<style lang="scss" scoped>
.playWnd {
  width: 100%;
  height: 100%;
  background: #262626;
}
</style>

```

## 调用方式

```vue
<template>
    <div id="home">
      <div class="container">
        <videoJsPlugin
          :camera-index-code="cameraIndexCode"
          :video-config="videoConfig"
        />
      </div>
    </div>
</template>

<script>
import videoJsPlugin from '@/components/tools/videoJsPlugin'
export default {
  name: 'index',
  components: {
    videoJsPlugin
  },
  data () {
    return {
      videoConfig: {
        ip: '10.193.74.250', // 综合安防管理平台IP地址
        appkey: '24398832', // 综合安防管理平台提供的appkey
        secret: 'pxaYtJc3p0vmMTrAFT3w', // 综合安防管理平台提供的secret
        playMode: 1
      },
      cameraIndexCode: '52353fbb8a774fb69dec56f2a60c742a'
    }
  }
}
</script>
<style scoped lang="scss">
#home{
  width: 100%;
  height: 100%;
  background-color: gray;
  overflow-y: scroll;
  .container{
    width: 800px;
    height: 400px;
    position: absolute;
    background-color: #fff;
  }
}
</style>

```

## 视频web插件功能说明及问题排查指南

[官方解答wiki](https://wiki.hikvision.com.cn/pages/viewpage.action?pageId=155112238)