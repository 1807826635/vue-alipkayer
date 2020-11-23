# vue-alipkayer
一个封装了阿里播放器的组件 开箱即用 解决了很多的bug


#基本用法
import alipkayer from '@/components/frontZone/alipkayer'
  components: {
    alipkayer
  }
}

  <alipkayer  :isLive='isLive' height="502px" :error="onReady" ref="myVideoKanBan" :source='source' @error="onReady"  @canplay="canplay"
                     :autoplay='false' class="video-components"></alipkayer>
   

   暴漏的方法可根据自己的需求使用  
   只有播放流可播放  才会走canplay这个函数
   
