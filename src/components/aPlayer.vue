<template>
  <div id="aplayer" ref="playerRef"></div>
</template>
<script>
import { defineComponent, toRefs, ref, reactive, onMounted, watch, computed } from "vue"
import APlayer from "aplayer";
import { useStore } from 'vuex'
import "aplayer/dist/APlayer.min.css";
export default defineComponent({
  name: "APlayer",
  setup () {
    const playerRef = ref()
    const state = reactive({
      instance: null,
    })
    const store = useStore()
    // APlayer歌曲信息
    class Audio {
      // 音频艺术家
      // artist: String;
      // 音频名称
      // name: String;
      // 音频链接
      // url: String;
      // 音频封面
      // cover: String;
      // 歌词
      // lrc: String;

      constructor(artist, name, url, cover, lrc) {
        this.artist = artist;
        this.name = name;
        this.url = url;
        this.cover = cover;
        this.lrc = lrc;
      }
    }
    const props = reactive({
      // 开启吸底模式
      // fixed: {
      //   type: Boolean,
      //   default: false
      // },
      // // 开启迷你模式
      // mini: {
      //   type: Boolean,
      //   default: false
      // },
      // // 音频自动播放
      autoplay: {
        type: Boolean,
        default: false
      },
      // 主题色
      theme: {
        type: String,
        default: "rgba(255,255,255,0.2)"
      },
      // 音频循环播放
      loop: {
        type: String,
        default: 'one' //'all' | 'one' | 'none'
      },
      // 音频循环顺序
      order: {
        type: String,
        default: 'random' //'list' | 'random'
      },
      // // 预加载
      preload: {
        type: String,
        default: 'auto' //'auto' | 'metadata' | 'none'
      },
      // // 默认音量
      // volume: {
      //   type: Number,
      //   default: 0.7,
      //   validator: (value) => {
      //     return value >= 0 && value <= 1;
      //   }
      // },
      // // 歌曲服务器(netease-网易云, tencent-qq音乐, kugou-酷狗, xiami-小米音乐, baidu-百度音乐)
      // songServer: {
      //   type: String,
      //   default: 'netease' //'netease' | 'tencent' | 'kugou' | 'xiami' | 'baidu'
      // },
      // // 播放类型(song-歌曲, playlist-播放列表, album-专辑, search-搜索, artist-艺术家)
      // songType: {
      //   type: String,
      //   default: 'playlist'
      // },
      // // 歌的id
      // songId: {
      //   type: String,
      //   default: '19723756'
      // },
      // // 互斥，阻止多个播放器同时播放，当前播放器播放时暂停其他播放器
      mutex: {
        type: Boolean,
        default: true
      },
      // 传递歌词方式
      // lrcType: {
      //   type: Number,
      //   default: 1
      // },
      // 列表是否默认折叠
      listFolded: {
        type: Boolean,
        default: true
      },
      // 列表最大高度
      listMaxHeight: {
        type: String,
        default: '100%'
      },
      // // 存储播放器设置的 localStorage key
      // storageName: {
      //   type: String,
      //   default: 'aplayer-setting'
      // },
    })
    // const lyric = computed({
    //   get: () => store.getters.lyric
    // })
    const songData = computed(() => store.getters.songData)
    watch(
      () => songData,
      (newValue, oldValue) => {
        let audioList = new Audio(newValue._value.author, newValue._value.title, newValue._value.url, newValue._value.pic) // lyric.value
        state.instance = new APlayer({
          container: playerRef.value,
          // fixed: props.fixed,
          // mini: props.mini,
          autoplay: props.autoplay,
          theme: props.theme,
          // loop: props.loop,
          // order: props.order,
          preload: props.preload,
          // volume: props.volume,
          mutex: props.mutex,
          // lrcType: props.lrcType,
          // listFolded: props.listFolded,
          // listMaxHeight: props.listMaxHeight,
          // storageName: props.storageName,
          audio: audioList
        })
      },
      { deep: true }
    )
    const songAllUrl = computed(() => store.getters.songAllUrl)
    watch(
      () => songAllUrl,
      (newValue, oldValue) => {
        state.instance = new APlayer({
          container: playerRef.value,
          autoplay: props.autoplay,
          theme: props.theme,
          preload: props.preload,
          mutex: props.mutex,
          listFolded: props.listFolded,
          listMaxHeight: props.listMaxHeight,
          audio: newValue._value
        })
      },
      { deep: true }
    )
    onMounted(() => {
      state.instance = new APlayer({
        container: playerRef.value,
        fixed: props.fixed,
        theme: props.theme,
      })
    })
    return {
      ...toRefs(state),
      ...toRefs(props),
      playerRef,
      props
    }

  }
})
</script>
<style lang="scss" scoped>
@media (min-width: 768px) {
  .aplayer::v-deep {
    position: relative;
    --tw-bg-opacity: 0;
    background-color: rgb(17 24 39 / var(--tw-bg-opacity));
    margin: 0px;

    // border: 1px solid #e0e0e0;
    // border-radius: 4px;
    .aplayer-body {
      .aplayer-info {
        .aplayer-music {
          .aplayer-title {
            --tw-text-opacity: 1;
            color: rgb(225 29 72 / var(--tw-text-opacity));
          }

          .aplayer-author {
            --tw-text-opacity: 1;
            color: rgb(225 29 72 / var(--tw-text-opacity));
          }
        }

        .aplayer-played {
          --tw-text-opacity: 1;
          background-color: rgb(225 29 72 / var(--tw-text-opacity));

          .aplayer-thumb {
            --tw-text-opacity: 1;
            background-color: rgb(225 29 72 / var(--tw-text-opacity));
          }
        }

        .aplayer-volume {
          --tw-text-opacity: 1;
          background-color: rgb(225 29 72 / var(--tw-text-opacity));
        }
      }
    }

    .aplayer-list {
      height: 290px;
      overflow-y: scroll;
      position: fixed;
      top: -18rem;
      right: 13rem;
      width: 23rem;
      background-color: rgb(17 24 39);

      ol {
        li {
        border-top: 1px solid #001529;
        --tw-text-opacity: 1;
        color: rgb(225 29 72 / var(--tw-text-opacity));
        .aplayer-list-light{
          background-color: skyblue !important;
        }
        .aplayer-list-index {
          --tw-text-opacity: 1;
          color: rgb(225 29 72 / var(--tw-text-opacity));
        }
        .aplayer-list-author {
          --tw-text-opacity: 1;
          color: rgb(225 29 72 / var(--tw-text-opacity));
        }
      }
      }
    }

    .aplayer-list::-webkit-scrollbar {
      width: 6px;
    }

    .aplayer-list::-webkit-scrollbar-thumb {
      z-index: 999;
      --tw-text-opacity: 1;
      background: rgb(225 29 72 / var(--tw-text-opacity));
      border-radius: 10px;
      // box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
    }

    .aplayer-list::-webkit-scrollbar-track {
      // box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
      border-radius: 0;
      --tw-bg-opacity: 1;
      background-color: rgb(17 24 39 / var(--tw-bg-opacity));
    }
  }
}
</style>