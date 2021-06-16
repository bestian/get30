<template>
  <q-page class="containter">
    <h3>搶三十</h3>
    <h4>誰先拿到第三十個棋子就勝利</h4>
    <div class="stone" v-for="i in items" :key="i" :class="{red: now == i || loose && i == items[items.length-1], green: last == i}">
      {{ i }}
    </div>
    <q-btn @click="take(1)" color = "primary" >拿一個棋子</q-btn>
    <q-btn @click="take(2)" color = "secondary" >拿兩個棋子</q-btn>
    <div v-if = "showBot">機器人拿了{{showBot}}個棋子</div>
    <q-btn color = "primary" @click="reset()" v-if="win">你勝利了，按此重來</q-btn>
    <q-btn color = "secondary" @click="reset()" v-if="loose">你輸了，按此重來</q-btn>

    <div>目前拿到第{{now}}顆</div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  props: ['max'],
  data() {
    return {
      items: [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30],
      now: 0,
      last: 0,
      nowP: 0,
      showBot: null
    }
  },
  methods: {
    take(n) {
      this.now += n
      this.last = this.now + 0
      if (this.now == this.items[this.items.length - 1]) {
        this.win = true
        this.loose = false
      }
      this.nowP++
      if (this.now % 3 == (this.items.length-2) % 3) {
        this.botTake(2)
      } else {
        if (this.now % 3 == (this.items.length-1) % 3) {
          this.botTake(1)
        }
        else {
          if (this.now % 3 == this.items.length % 3) {
            this.botTake(Math.floor(Math.random()*2) + 1)
          }
        }
      } 
    },
    botTake(n) {
      this.now += n
      this.showBot = n + 0
      if (this.now == this.items[this.items.length - 1]) {
        this.win = false
        this.loose = true
      }
      this.nowP--
    },
    reset() {
      this.win = false
      this.loose = false
      this.now = 0
      this.last = 0
      this.botTake(Math.floor(Math.random()*2) + 1)
      this.items = []
      for (var i = 1; i <= this.max; i++) {
        this.items.push(i)
      }
    }
  },
  mounted() {
    this.reset()
  },
  watch: {
    max() {
      this.reset()
    }
  }
}
</script>

<style type="text/css" scoped="">

  .stone {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    border: 1px solid black;
    border-radius: 15px;
    font-size: 16px;
    font-weight: bolder;
  }

  .red {
    color: red;
  }

  .green {
    color: green;
  }

  .containter {
    max-width: 800px;
    margin: 0 auto;
  }
</style>
