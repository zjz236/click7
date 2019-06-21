<template>
    <div id="game">
      <div class="score-border">
        <div class="score">
          <div class="scoreNum">
            <div class="now">
              <h3 class="scoreName">目前得分</h3>
              <h2 class="sc">{{nowScore}}</h2>
            </div>
            <div class="history">
              <h3 class="scoreName">历史最高分</h3>
              <h2 class="sc">{{historyScore}}</h2>
            </div>
          </div>
          <div class="num">
            <h2 class="numName">敲7计数</h2>
            <h1 class="number">{{num}}</h1>
          </div>
          </div>
        </div>
        <div class="bt">
          <div class="click">
            <el-button circle @click="timeOn" :disabled="start">开始</el-button>
          </div>
          <div class="click">
            <el-button circle @click="clickSeven" :disabled="click">敲7</el-button>
          </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'index',
  data () {
    return {
      timer: null,
      num: 1,
      autoplay: false,
      sevennum: 1,
      historyScore: 0,
      nowScore: 0,
      seven: [],
      timer2: null,
      verdict: false,
      start: false,
      click: true
    }
  },
  methods: {
    numChange () {
      this.click = false
      this.num++
      this.sevennum++
      if (this.num > 10000000) {
        this.perfect()
      }
      if (this.sevennum > 70) this.sevennum %= 70
      if (this.verdict === false && !this.judge(this.num - 1, this.sevennum - 1) === false && this.num > 1 && this.sevennum > 1) {
        this.clickFail()
      }
      this.verdict = false
      if (this.num % 10 === 0 && this.num < 500) {
        let time = 1000 - this.num
        this.timeChange(time)
      }
    },
    perfect () {
      clearInterval(this.timer)
      this.start = false
      this.click = true
      this.$alert('<h1>您的最终分数</h1><h1>' + this.nowScore + '</h1>', '满分了', {
        dangerouslyUseHTMLString: true
      })
      if (this.nowScore > this.historyScore) {
        this.historyScore = this.nowScore
        localStorage.setItem('historyScore', this.$Base64.encode(this.historyScore))
      }
      this.nowScore = 0
      this.num = this.sevennum = 1
    },
    timeChange (time) {
      clearInterval(this.timer)
      this.timer = setInterval(this.numChange, time)
    },
    timeOn () {
      this.start = true
      this.click = false
      this.timer = setInterval(this.numChange, 1000)
    },
    judge (num, sevennum) {
      var number = num + ''
      if (number.indexOf('7') !== -1 || this.seven[sevennum] === 1) {
        return true
      }
      return false
    },
    clickSeven () {
      this.click = true
      if (this.judge(this.num, this.sevennum)) {
        this.verdict = true
        this.nowScore++
      } else {
        this.clickFail()
      }
    },
    clickFail () {
      clearInterval(this.timer)
      this.start = false
      this.click = true
      if (this.nowScore > this.historyScore) {
        this.historyScore = this.nowScore
        localStorage.setItem('historyScore', this.$Base64.encode(this.historyScore))
      }
      this.$alert('<h1>您的最终分数</h1><h1>' + this.nowScore + '</h1>', this.nowScore > this.historyScore ? '恭喜您突破记录' : '失败了', {
        dangerouslyUseHTMLString: true
      })
      this.nowScore = 0
      this.num = this.sevennum = 1
    }
  },
  created () {
    this.seven.push(0)
    for (var i = 1; i <= 70; i++) {
      if (i % 7 === 0) {
        this.seven.push(1)
      } else {
        this.seven.push(0)
      }
    }
    let data = localStorage.getItem('historyScore')
    if (data == null) {
      localStorage.setItem('historyScore', this.$Base64.encode(0))
      this.historyScore = 0
    } else {
      this.historyScore = this.$Base64.decode(data)
    }
  }
}
</script>

<style scoped>
  .score-border{
    background:#878787;
    width: 450px;
    height: 250px;
    margin: 0 auto;
    border-left: 1px solid #000;
    display: flex;
    align-items: flex-end;
    justify-content: center;
    border-right: 1px solid #000;
  }
  .score{
    background: #FFFF00;
    width: 95%;
    border: 1px solid #000;
    height: 230px;
  }
  .scoreNum{
    display: flex;
    height: 40%;
    align-items: center;
    justify-content: center;
    flex-direction: row;
  }
  .scoreNum div{
    display: flex;
    width: 50%;
    align-items: center;
    flex-direction: column;
    justify-content: center;
  }
  .scoreName {
    color: #ff0000;
  }
  .sc{
    background: #3B3B3B;
    width: 50%;
    text-align: right;
    padding: 0 5px 0 5px;
    margin-top: 0px;
    color: #fff;
    font-family: UnidreamLED;
  }
  .num{
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
  }
  .numName {
    color: #ff0000;
  }
  .number{
    background: #3B3B3B;
    color: #fff;
    width: 40%;
    font-family: UnidreamLED;
    margin-top: 0px;
    padding: 0 10px 0 10px;
    height: 50px;
    line-height: 50px;
    text-align: right;
  }
  .bt{
    width: 450px;
    height: 250px;
    background: #FFA500;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: row;
    border: 1px solid #000;
  }
  .click{
    width: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .click button{
    height: 100px;
    font-size: 25px;
    background: #ff0000;
    border: none;
    color: #fff;
    width: 100px;
  }
  .el-button.is-disabled, .el-button.is-disabled:focus, .el-button.is-disabled:hover {
    color: #fff;
    cursor: not-allowed;
    background-image: none;
    background-color: #FF0000;
    border:none;
  }
</style>
