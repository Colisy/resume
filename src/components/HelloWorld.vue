<template>
<div class="hello">
  <Remuse :code='code'></Remuse>
</div>
</template>

<script>
import Remuse from "./remuse"
export default {
  name: 'HelloWorld',
  components: {
    Remuse
  },
  data() {
    return {
      code: "",
      currentStyle: [
        `/*
* 大家好，我是李珊珊
* 马上快到年底了，很多人也想换一份工作，你是不是也在准备简历呀。
* 说做就做，我也来写一份简历！
*/

/* 首先给所有元素加上过渡效果 */
* {
transition: all .3s;
}
/* 白色背景太单调了，我们来点背景 */
html {
color: #fff; background: #D47575;
}
/* 文字离边框太近了 */
.styleEditor {
padding: .5em;
border: 1px solid;
margin: .5em;
overflow: auto;
width: 45vw; height: 90vh;
}
/* 代码高亮 */
.token.selector{ color: rgb(133,153,0); }
.token.property{ color: rgb(187,137,0); }
.token.punctuation{ color: yellow; }
.token.function{ color: rgb(42,161,152); }

/* 加点 3D 效果呗 */
html{
perspective: 1000px;
}
.styleEditor {
position: fixed; left: 0; top: 0;
-webkit-transition: none;
transition: none;
-webkit-transform: rotateY(10deg) translateZ(-100px) ;
        transform: rotateY(10deg) translateZ(-100px) ;
}

/* 接下来我给自己准备一个编辑器 */
.resumeEditor{
position: fixed; right: 0; top: 0;
padding: .5em;  margin: .5em;
width: 48vw; height: 90vh;
border: 1px solid;
background: white; color: #222;
overflow: auto;
}
/* 好了，我开始写简历了 */


`,
        `
/* 再对 HTML 加点样式 */
.resumeEditor{
padding: 2em;
}
.resumeEditor h2{
display: inline-block;
border-bottom: 1px solid;
margin: 1em 0 .5em;
}
.resumeEditor ul,.resumeEditor ol{
list-style: none;
}
.resumeEditor ul> li::before{
content: '•';
margin-right: .5em;
}
.resumeEditor ol {
counter-reset: section;
}
.resumeEditor ol li::before {
counter-increment: section;
content: counters(section, ".") " ";
margin-right: .5em;
}
.resumeEditor blockquote {
margin: 1em;
padding: .5em;
background: #ddd;
}
`
],
index:0
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      // 获取数组的长度
      var length = this.currentStyle.filter((_, index) => index <= 0).map((item) => item.length).reduce((p, c) => p + c, 0)
        var str = this.currentStyle[0];
       var i = 0;
       var that=this;
       // 打字机效果
       var showStyle=(function(){
         var divTyping = that.code;
         if (i <= str.length) {
          that.code= str.slice(0, i++) + '_';
          setTimeout(showStyle, 200);//递归调用
         }
         else{
          that.code = str;//结束打字,移除 _ 光标
         }

       })
       showStyle()
    }
  }
}
</script>

<style scoped>

</style>
