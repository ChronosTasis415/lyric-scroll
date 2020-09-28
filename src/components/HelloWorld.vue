<template>
  <div class="video-content-box">
    <video ref="video-player" @timeupdate="onTimeUpdate" class="video-player" controls>
      <source src="../assets/demo.mp4" type="video/mp4">
    </video>
    <div class="lyric">
      <LyricScroll
        :lyric="lyric"
        :t-lyric="tLyric"
        :lyric-active-class="'lyric-active'"
        :lyric-center-class="'lyric-center'"
        triangle-width="14px"
        triangle-color="#fff"
        center-line-color="#fff"
        center-time-color="#fff"
        :current-time="currentTime"
        @change-current-time="handleChangeCurrentTime"
      />
    </div>
  </div>
</template>

<script>
import LyricScroll from './LyricScroll';

export default {
  name: 'HelloWorld',
  components: {
    LyricScroll
  },
  data: () => (
    {
      videoObj: null,
      playUrl: '',
      videoContext: null,
      // 原始歌词
      originLyric: '',
      // 原始译词
      originTLyric: '',
      // 当前播放时间
      currentTime: 0
    }
  ),
  computed: {
    // 原词，格式为{xx: 歌词, ...}，xx为该词开始时间
    lyric() {
      return this.lyricToObj(this.originLyric)
    },
    // 译词，格式同原词
    tLyric() {
      return this.lyricToObj(this.originTLyric)
    }
  },
  methods: {
    jump(time) {
      this.videoObj.currentTime = time;
    },
    onTimeUpdate(e) {
      // this.jump(e.target.currentTime);
      this.currentTime = e.target.currentTime;
    },
    // 将00:00.00转换为秒数
    timeStrToNum(str) {
      const minute = Number(str.slice(0, 2))
      const second = Number(str.slice(3, 5))
      const minSec = Number(str.slice(6, 8))
      return minute * 60 + second + minSec / 100
    },
    // 将歌词字符串转换为对象，格式为{开始时间: 歌词, ...}
    lyricToObj(lyricStr) {
      const obj = {}
      let perLyric
      let time
      lyricStr.split('\n').forEach((item) => {
        perLyric = item.slice(item.indexOf(']') + 1)
        if (perLyric) {
          time = this.timeStrToNum(item.slice(1, 9))
          obj[time] = perLyric
        }
      })
      return obj
    },

    handleChangeCurrentTime(time) {
      console.log(time);
      this.jump(time);
    }
  },
  mounted() {
    this.videoObj = this.$refs['video-player'];
    this.originLyric =
      '[00:00.100]仆がずっと前から思ってる事を话そうか\n[00:02.250]友达に戻れたらこれ以上はもう望まないさ\n[00:03.020]君がそれでいいなら仆だってそれで构わないさ\n[00:04.010]嘘つきの仆が吐いた はんたいことばの爱のうた\n[00:06.360]\n[00:08.820]今日はこっちの地方はどしゃぶりの晴天でした\n[00:10.690]昨日もずっと暇で一日満喫してました\n[00:14.560]别に君のことなんて考えてなんかいないさ\n[00:17.530]いやでもちょっと本当は考えてたかもなんて\n[00:24.510]メリーゴーランドみたいに\n[00:29.240]仆の头ん中はもうグルグルさ\n[00:34.760]\n'
    this.originTLyric =
      '[00:00.100]我来告诉你我一直以来所想的事吧\n[00:02.250]能回到朋友关系的话我就别无所求了\n[00:03.020]如果你无所谓的话那我也没关系\n[00:04.010]骗子的我所吐出的　相反词的情歌\n[00:06.820]今日附近地区是倾盆大雨的大晴天\n[00:08.690]昨天也是一整天都很闲的充实一天\n[00:10.690]我又没有在想你的事情\n[00:14.560]不对 其实搞不好有想那麽一点点\n[00:17.530]你所给的爱要扔到哪好？\n[00:24.510]像旋转木马那样旋转\n[00:29.240]我的脑袋也转啊转的\n[00:35.310]几乎要自两手中洒出的\n'
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.video-content-box {
  width: 100%;
  height: 50%;
  font-size: 16px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #ffffff;
  margin: 0 auto;
  .video-player {
    flex-shrink: 0;
    width: 300px;
    height: 150px;
    object-fit: cover;
  }
  .audio-wrapper {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .lyric {
    width: 100%;
    color: #adaaa8;
    box-sizing: border-box;
    border: 1px dashed #ccc;
    box-sizing: border-box;
    text-align: center;
  }
  .lyric-active {
    // 需要最高级，否则可能被内部的.center-lyric覆盖
    color: #fff !important;
  }
  .lyric-center {
    color: #fff;
  }
}
</style>
