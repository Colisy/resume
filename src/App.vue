<template>
  <div id="app">
    <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
    <ResumeEditor ref="resumeEditor" :markdown="currentMarkdown" :enableHtml="enableHtml"></ResumeEditor>
  </div>
</template>

<script>
  import StyleEditor from './components/StyleEditor'
  import ResumeEditor from './components/ResumeEditor'
  import './assets/reset.css'

  export default {
    name: 'app',
    components: {
      StyleEditor,
      ResumeEditor
    },
    data() {
      return {
        interval: 50,
        currentStyle: '',
        enableHtml: false,
        fullStyle: [  ``],
        currentMarkdown: '',
        fullMarkdown: `
  您好，我是李珊珊，
  现任职于：北京飞猪星球文化有限公司。
  很荣幸有机会让您阅览我的简历，
  接下来请允许我做一个自我介绍
  李珊珊（Colisy）
----
* 前端小白一枚，一入前端深似海，从此沉迷不自拔。
* 性别： 女
* 年龄： 24


技能
----

* 前端基础开发（html、css、js、jquery）
* 前端框架使用（vue、react做项目ok，看源码有待提高）
* Node.js 开发 （不太精通，仅限于express+mongodb+nodejs数据库实现增删改查、并返回给前台所需数据、其他类似爬虫方面不精通）
* 前端开发方面（pc端、m站、公众号、小程序，没开发过app）

工作经历
----

1. 北京飞猪星球文化有限公司 （音乐类型的项目开发）
2. 长行时代科技有限公司 （商城方面的项目开发）
3. 居然之家投资控股集团总部（实习） （商城方面的项目开发）

联系方式
----
* 邮箱：347996377@qq.com
* 电话号： 18401466210
* 微信: l347996377 或 15313145474

链接
----

* [GitHub](https://github.com/Colisy)
* [博客](https://colisy.github.io)
* [简书](https://www.jianshu.com/u/129d3813274d)
* [知乎](https://www.zhihu.com/people/xiao-huai-99/activities)

`
      }
    },
    created() {
      this.makeResume()
    },
    methods: {
      makeResume: async function () {
        await this.showHtml()
        await this.progressivelyShowResume(0)
      },
      showHtml: function () {
        return new Promise((resolve, reject) => {
          this.enableHtml = true
          resolve()
        })
      },
      progressivelyShowStyle(n) {
        return new Promise((resolve, reject) => {
          let interval = this.interval
          let showStyle = (async function () {
            let style = this.fullStyle[n]
            if (!style) { return }
            // 计算前 n 个 style 的字符总数
            let length = this.fullStyle.filter((_, index) => index <= n).map((item) => item.length).reduce((p, c) => p + c, 0)
            let prefixLength = length - style.length
            if (this.currentStyle.length < length) {
              let l = this.currentStyle.length - prefixLength
              let char = style.substring(l, l + 1) || ' '
              this.currentStyle += char
              if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
                this.$nextTick(() => {
                  this.$refs.styleEditor.goBottom()
                })
              }
              setTimeout(showStyle, interval)
            } else {
              resolve()
            }
          }).bind(this)
          showStyle()
        })
      },
      progressivelyShowResume() {
        return new Promise((resolve, reject) => {
          let length = this.fullMarkdown.length
          let interval = this.interval
          let showResume = () => {
            if (this.currentMarkdown.length < length) {
              this.currentMarkdown = this.fullMarkdown.substring(0, this.currentMarkdown.length + 1)
              let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
              let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
              if (prevChar === '\n' && this.$refs.resumeEditor) {
                this.$nextTick(() => this.$refs.resumeEditor.goBottom())
              }
              setTimeout(showResume, interval)
            } else {
              resolve()
            }
          }
          showResume()
        })
      }
    }
  }

</script>

<style scoped>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  html {
    min-height: 100vh;
  }
  *{
    box-sizing: border-box;
  }
</style>
