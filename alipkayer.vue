<template>
  <div id='videomonitoring'>
    <script type="text/javascript" charset="utf-8" src="https://g.alicdn.com/de/prismplayer/2.9.1/aliplayer-min.js"></script>
    <div>
      <div class='box-card'>
        <div style='height:560px'>
          <div class='prism-player' :id='playerId' :style='playStyle'></div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
/* eslint-disable */
  export default {
    // 在vue中怎么使用Aliplayer
    name: 'Aliplayer',
    props: {
      playStyle: {
        type: String,
        default: ''
      },
      // aliplayerSdkPath: {
      //   // Aliplayer 代码的路径
      //   type: String,
      //   default: 'https://g.alicdn.com/de/prismplayer/2.9.1/aliplayer-min.js'
      // },
      autoplay: {
        type: Boolean,
        default: true
      },
      isLive: {
        type: Boolean,
        default: false
      },
      playsinline: {
        // H5是否内置播放，有的Android浏览器不起作用。
        type: Boolean,
        default: false
      },
      width: {
        type: String,
        default: '100%'
      },
      height: {
        type: String,
        default: '560px'
      },
      controlBarVisibility: {
        // 控制面板的实现，默认为‘hover’， 可选的值为：‘click’、‘hover’、‘always’。
        type: String,
        default: 'hover'
      },
      useH5Prism: {
        type: Boolean,
        default: false
      },
      useFlashPrism: {
        type: Boolean,
        default: false
      },
      vid: {
        type: String,
        default: ''
      },
      playauth: {
        type: String,
        default: ''
      },
      source: {
        type: String,
        default: ''
      },
      cover: {
        type: String,
        default: ''
      },
      format: {
        type: String,
        default: 'm3u8'
      },
      skinLayout: {
        type: Array,
        default: function () {
          return []
        }
      },
      x5_video_position: {
        type: String,
        default: 'top'
      },
      x5_type: {
        type: String,
        default: 'h5'
      },
      x5_fullscreen: {
        type: Boolean,
        default: false
      },
      x5_orientation: {
        type: Number,
        default: 2
      },
      autoPlayDelay: {
        type: Number,
        default: 0
      },
      autoPlayDelayDisplayText: {
        type: String
      }
    },
    data () {
      return {
        playerId:
        'aliplayer_' +
        Math.random()
          .toString(36)
          .substr(2),
        scriptTagStatus: 0,
        isReload: false,
        instance: null,
        tiems:0
      }
    },
    created () {

    },
    mounted () {
      this.checked()
    },
    methods: {
      checked(){
        if (window.Aliplayer === undefined) {
          if(this.tiems === 5){
            return false
          }
          this.sleep(1000).then(()=>{
            this.tiems += 1
            this.checked()
          })
        }else{
          this.initAliplayer()
        }
      },
      sleep(tiem){
        return new Promise((resolve, reject)=>{
          setTimeout(function(){
            resolve();
          }, tiem);
        })
      },
      initAliplayer () {
        const _this = this
        _this.instance = window.Aliplayer({
          id: _this.playerId,
          autoplay: _this.autoplay,
          isLive: _this.isLive,
          playsinline: _this.playsinline,
          format: _this.format,
          width: _this.width,
          height: _this.height,
          controlBarVisibility: _this.controlBarVisibility,
          useH5Prism: _this.useH5Prism,
          useFlashPrism: _this.useFlashPrism,
          vid: _this.vid,
          playauth: _this.playauth,
          source: _this.source,
          cover: _this.cover,
          'skinLayout': [
            {
              'name': 'bigPlayButton',
              'align': 'blabs',
              'x': 30,
              'y': 80
            },
            {
              'name': 'H5Loading',
              'align': 'cc'
            },
            {
              'name': 'errorDisplay',
              'align': 'tlabs',
              'x': 0,
              'y': 0
            },
            {
              'name': 'infoDisplay'
            },
            {
              'name': 'tooltip',
              'align': 'blabs',
              'x': 0,
              'y': 56
            },
            {
              'name': 'thumbnail'
            },
            {
              'name': 'controlBar',
              'align': 'blabs',
              'x': 0,
              'y': 0,
              'children': [
                {
                  'name': 'progress',
                  'align': 'blabs',
                  'x': 0,
                  'y': 44
                },
                {
                  'name': 'playButton',
                  'align': 'tl',
                  'x': 15,
                  'y': 12
                },
                {
                  'name': 'timeDisplay',
                  'align': 'tl',
                  'x': 10,
                  'y': 7
                },
                {
                  'name': 'fullScreenButton',
                  'align': 'tr',
                  'x': 10,
                  'y': 12
                },
                {
                  'name': 'volume',
                  'align': 'tr',
                  'x': 5,
                  'y': 10
                }
              ]
            }
          ],
          // skinLayout: _this.skinLayout, // 说明：功能组件布局配置，不传该字段使用默认布局传false隐藏所有功能组件，请参照皮肤定制
          x5_video_position: _this.x5_video_position,
          x5_type: _this.x5_type,
          x5_fullscreen: _this.x5_fullscreen,
          x5_orientation: _this.x5_orientation,
          autoPlayDelay: _this.autoPlayDelay,
          autoPlayDelayDisplayText: _this.autoPlayDelayDisplayText
        })
        // 绑定事件，当 AliPlayer 初始化完成后，将编辑器实例通过自定义的 ready 事件交出去
        _this.instance.on('ready', () => {
          this.$emit('ready', _this.instance)
        })
        _this.instance.on('play', () => {
          this.$emit('play', _this.instance)
        })
        _this.instance.on('pause', () => {
          this.$emit('pause', _this.instance)
        })
        _this.instance.on('canplay', (e) => {
          this.$emit('canplay', _this.instance)
        })
        _this.instance.on('ended', () => {
          this.$emit('ended', _this.instance)
        })
        _this.instance.on('liveStreamStop', () => {
          this.$emit('liveStreamStop', _this.instance)
        })
        _this.instance.on('m3u8Retry', () => {
          this.$emit('m3u8Retry', _this.instance)
        })
        _this.instance.on('hideBar', () => {
          this.$emit('hideBar', _this.instance)
        })
        _this.instance.on('waiting', () => {
          this.$emit('waiting', _this.instance)
        })
        _this.instance.on('snapshoted', () => {
          this.$emit('snapshoted', _this.instance)
        })

        _this.instance.on('timeupdate', () => {
          _this.$emit('timeupdate', _this.instance)
        })
        _this.instance.on('requestFullScreen', () => {
          _this.$emit('requestFullScreen', _this.instance)
        })
        _this.instance.on('cancelFullScreen', () => {
          _this.$emit('cancelFullScreen', _this.instance)
        })
        _this.instance.on('error', (e) => {
          console.log('sss')
          _this.$emit('error', _this.instance)
        })
        _this.instance.on('startSeek', () => {
          _this.$emit('startSeek', _this.instance)
        })
        _this.instance.on('completeSeek', () => {
          _this.$emit('completeSeek', _this.instance)
        })
      },
      /**
       * 播放视频
       */
      play: function () {
        this.instance.play()
      },
      /**
       * 暂停视频
       */
      pause: function () {
        if (this.instance) {
          this.instance.pause()
        } else {
          return false
        }
      },
      /**
       * 重播视频
       */
      replay: function () {
        this.instance.replay()
      },
      // 能够播放
      canplay: function (e) {
        this.instance.canplay(e)
      },
      /**
       * 跳转到某个时刻进行播放
       * @argument time 的单位为秒
       */
      seek: function (time) {
        this.instance.seek(time)
      },
      /**
       * 获取当前时间 单位秒
       */
      getCurrentTime: function () {
        return this.instance.getCurrentTime()
      },
      /**
       *获取视频总时长，返回的单位为秒
       * @returns 返回的单位为秒
       */
      getDuration: function () {
        return this.instance.getDuration()
      },
      /**
       获取当前的音量，返回值为0-1的实数ios和部分android会失效
       */
      getVolume: function () {
        return this.instance.getVolume()
      },
      /**
       设置音量，vol为0-1的实数，ios和部分android会失效
       */
      setVolume: function (vol) {
        this.instance.setVolume(vol)
      },
      /**
       *直接播放视频url，time为可选值（单位秒）目前只支持同种格式（mp4/flv/m3u8）之间切换暂不支持直播rtmp流切换
       *@argument url 视频地址
       *@argument time 跳转到多少秒
       */
      loadByUrl: function (url, time) {
        this.instance.loadByUrl(url, time)
      },
      /**
       * 设置播放速度
       *@argument speed 速度
       */
      setSpeed: function (speed) {
        this.instance.setSpeed(speed)
      },
      /**
       * 设置播放器大小w,h可分别为400px像素或60%百分比chrome浏览器下flash播放器分别不能小于397x297
       *@argument w 播放器宽度
       *@argument h 播放器高度
       */
      setPlayerSize: function (w, h) {
        this.instance.setPlayerSize(w, h)
      },
      /**
       * 目前只支持HTML5界面上的重载功能,暂不支持直播rtmp流切换m3u8）之间切换,暂不支持直播rtmp流切换
       *@argument vid 视频id
       *@argument playauth 播放凭证
       */
      reloaduserPlayInfoAndVidRequestMts: function (vid, playauth) {
        this.instance.reloaduserPlayInfoAndVidRequestMts(vid, playauth)
      },
      disposePlayer () {
        this.instance.dispose()
      },
      reloadPlayer: function () {
        this.isReload = true
        this.initAliplayer()
        this.isReload = false
      }
    },
    beforeDestroy(){
      console.log('销毁')
      this.instance.shows=true
      if (this.instance) {
        this.instance && this.instance.dispose()
      }
      this.instance.shows=false
    },
    watch: {
      source: {
        deep: true, // 深度监听
        handler (newVal, oldVal) {
          if (!this.source) {
            return false
          } else{
            this.disposePlayer()
            this.source = newVal
            this.initAliplayer()
          }
        }
      },
      isLive:{
        deep: true, // 深度监听
        handler (newVal, oldVal) {
          this.isLive = newVal
        }
      }
    }
  }
</script>

<style>
  @import './../../assets/styles/aliplayer-min.css';
  #videomonitoring {
    width: 100%;
    border-radius: 10px;
    overflow: hidden;
  }
  #videomonitoring > div {
    height: 100%;
  }
</style>
