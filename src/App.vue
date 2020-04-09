<template>
  <div id="app">
    <div class="lesson1">
      <h2>v-show・v-ifの使い分け</h2>
      <button @click="isVif=!isVif">v-ifの切替</button>
      <div v-if="isVif">v-ifがtrueだよ</div>
      <div v-else>v-ifがfalseだよ</div>
      <br />
      <button @click="isVshow=!isVshow">v-showの切替</button>
      <div v-show="isVshow">v-showがtrueだよ</div>
      <div v-show="!isVshow">v-showがfalseだよ</div>
      <br />
      <div>Q1 違いは何か？</div>
      <button @click="answer1=!answer1">答え1</button>
      <div v-show="answer1">v-ifは条件に合うDOMを作成し、条件に合わないDOMを消す<br />v-showは条件に合わない要素に「display: none;」を付与し、CSSの切替によって実現する。（DOMとしては常に存在する）</div>
      <div>Q2 どう使い分けるべき？</div>
      <button @click="answer2=!answer2">答え2</button>
      <div v-show="answer2">v-ifは初期表示のコストが低い代わりに、表示切替のコストが高い<br />v-showは初期表示のコストが高い代わりに表示切替のコストが低い<br />頻繁に切替が発生するようならv-showを使うのも手<br />※ただしSafari, IEでは仕様で「display: none;」のようなdisplayプロパティが適用できない<br />なのでplus_clinicでは基本的にv-ifを使用するという事でいいのかも（IEで上手く表示されないのはdisplayプロパティを使用しているから？？？）</div>
    </div>
    <div class="lesson2">
      <h2>computed・methodsの使い分け</h2>
      <button @click="isShow=!isShow">表示/非表示</button>
      <div v-if="isShow">
        Computed: {{ nowComputed }}
        <br />
        Method: {{ nowMethod() }}
      </div>
      <div>Q3 特徴は？</div>
      <button @click="answer3=!answer3">答え3</button>
      <div v-show="answer3">Computedの特徴：計算結果をキャッシュに保存する。計算結果は依存するデータの値が更新されないとデータを返すだけ<br />computedは算出プロパティというプロパティであること。staffオブジェクトから苗字を表示させたい時、「staff.lastName」で「佐伯」と表示されるのと同じように「vm.nowComputed」と書いて表示しているようなイメージ。<br />returnが必要<br /><br />Methodの特徴：計算結果をキャッシュしておらず、DOMが再描画される度に計算しなおす。(それが対象っぽくなくてもどこかが再描画された時に発動する)<br />関数であること。なので計算結果を表示する場合、「nowMethod()」のように括弧が必要。</div>
      <div>Q4 どう使い分けるべき？</div>
      <button @click="answer4=!answer4">答え4</button>
      <div v-show="answer4">Computed:dataの変更に応じてリアクティブに表示を変更させたい時<br /><br />Method:dataの値以外の変更に応じてリアクティブに表示を変更させたい時(例：Date.now()など)、ボタンを押した、テキストボックスに入力をしたなど、アクションが起きた時に処理を行いたい場合<br />今まで通りでよさそう</div>
    </div>
    <div class="lesson3">
      <h2>computed・watchの使い分け</h2>
      <input type="number" v-model.number="price">円
      <div>{{ price }}円</div>
      <div>computed: {{ computedPriceMessage }}</div>
      <div>watch: {{ message }}</div>
      <div>watchはリアクティブなデータの変更が発生した時に非同期処理を実行したい場合に使うらしい。またはcomputedでは大きすぎる処理をしたい時に使うらしい。</div>
      <div>今のところ、watchはreturnをする必要がないことoldValueを取れる(取りやすい)事</div>
    </div>
    <div>
      <h2>その他</h2>
      <div>flgのtrue, falseの切替位ならmethodsを定義しなくても簡単にできる。（良いか悪いかは別として）</div>
      <div>v-modelにはnumber修飾子を噛ませないとstring型になる</div>
      <div>{{ typeof(price) }}</div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      isVif: true,
      isVshow: true,
      answer1: false,
      answer2: false,
      answer3: false,
      answer4: false,
      isShow: false,
      price: 0,
      message: '1000円以下です'
    }
  },
  methods: {
    nowMethod() {
      return Date.now()
    }
  },
  computed: {
    nowComputed() {
      return Date.now()
    },
    computedPriceMessage() {
      let computedPriceMessage = '1000円以下です'
      if (this.price >= 1000) {
        computedPriceMessage = '1000円以上です'
      }
      return computedPriceMessage
    }
  },
  watch: {
    price(newValue, oldValue) {
      console.log(`${newValue} ${oldValue}`)
      if(newValue >= 1000) {
        this.message = '1000円以上です'
      } else {
        this.message = '1000円以下です'
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
