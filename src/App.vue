<template>
  <div id="app">
    <div class="msg"><h1>{{ msg }}</h1></div>
    <div class="buttons display">
      <div>
        <p>{{ questionType }}</p>
        <h3>{{ answer }}</h3>
      </div>
    </div>
    <div class="buttons">
      <!-- <p class="notes" v-for="note in notes"></p> -->
    </div>
    <div class="buttons row1">
      <div>n: <input type="number" v-model="memory.n"></div>
      <div>iYr: <input type="number" v-model="memory.iYr"></div>
      <div>pv: <input type="number" v-model="memory.pv"></div>
      <div>pmt: <input type="number" v-model="memory.pmt"></div>
      <div>pYpmt: <input type="number" v-model="memory.pYpmt"></div>
      <div>fv: <input type="number" v-model="memory.fv"></div>
    </div>
    <div class="buttons row2">
      <div>
        <div>
          <button @click="calculateFV(memory.pv, memory.iYr, memory.n)">Calc FV</button>
          <button @click="calculatePV(memory.pv, memory.iYr, memory.n)">Calc PV</button>
        </div>
        <div>
          <button @click="calculateOrFV(memory.pmt, memory.iYr, memory.n)">Calc Ord FV</button>
          <button @click="calculateOrPV(memory.pmt, memory.iYr, memory.n)">Calc Ord PV</button>
        </div>
        <!-- <div>
          <button @click="calculateDuFV(memory.pmt, memory.iYr, memory.n)">Calc An Due FV</button>
          <button @click="calculateDuPV(memory.pmt, memory.iYr, memory.n)">Calc An Due PV</button>
        </div> -->
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      msg: 'Financial Calculator',
      questionType: '',
      answer: 0,
      notes: [],
      memory: {
        n: 0,
        iYr: 0,
        pv: 0,
        pmt: 0,
        pYpmt: 0,
        fv: 0
      }
    }
  },
  methods: {
    clog: function (item) {
      console.log(item)
    },
    lightNeeded: function () {

    },
    detectPercentage: function (iYr) {
      if (iYr > 1) {
        this.notes.push('iYr has been multiplied by 100 to equal %') 
        return  iYr / 100;
      }
    },
    detectZero: function (a, b, c, d) {
      var zeros = []
      if (a == 0) {
        zeros.push(Object.keys(a));
        console.log(zeros);
      }
      if (b == 0) {
        zeros.push(b);
        console.log(zeros);
      }
      if (c == 0) {
        zeros.push(c);
        console.log(zeros);
      }
      if (d == 0) {
        zeros.push(d);
        console.log(zeros);
      }
      this.notes.push(zeros + 'have been detected to be nill values')
    },
    rate: function (i, n) {
      return Math.pow((1 + i),n);
    },
    calculateFV: function (pv, iYr, n) {
      // this.detectZero(pv, iYr, n);
      iYr = this.detectPercentage(iYr);

      this.questionType = 'Future Value: '
      this.answer = (pv * this.rate(iYr,n));
    },
    calculatePV: function (fv, iYr, n) {
      // this.detectZero(fv, iYr, n);
      iYr = this.detectPercentage(iYr);
      
      this.questionType = 'Present Value: '
      this.answer = (fv / this.rate(iYr, n));
    },
    calculateOrFV: function (pmt, iYr, n) {
      // this.detectZero(pmt, iYr, n);
      iYr = this.detectPercentage(iYr);

      this.questionType = 'Ordinary Annuity Future Value: ';
      this.answer = (pmt/iYr) * (this.rate(iYr, n) - 1); 
    },
    calculateOrPV: function (pmt, iYr, n) {
      // this.detectZero(pmt, iYr, n);
      iYr = this.detectPercentage(iYr);

      this.questionType = 'Ordinary Annuity Future Value: ';
      this.answer = (pmt/iYr) * (1 - (1/this.rate(iYr, n)));
    },
    // calculateDuFV: function (pmt, iYr, n) {
    //   // this.detectZero(pmt, iYr, n);
    //   iYr = this.detectPercentage(iYr);

    //   this.questionType = 'Ordinary Annuity Future Value: ';
    //   this.answer = ((pmt/iYr) * (this.rate(iYr, n) - 1)*(1+iYr)); 
    // },
    // calculateDuPV: function (pmt, iYr, n) {
    //   this.detectZero(pmt, iYr, n);
    //   iYr = this.detectPercentage(iYr);
    //   var rate = Math.pow((1 + iYr), n);

    //   this.questionType = 'Ordinary Annuity Future Value: ';
    //   this.answer = ((pmt/i) * (1 - (1/rate))*(1+i));
    // },
  }
}
</script>

<style>
#app {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
  display: flex;
  flex-flow: column;
}
.buttons {
  border: solid red 1px;
  display: flex;
  flex-flow: row;
  flex-wrap: wrap;
  padding: 1em;
  align-items: center;
  justify-content: space-around;
}
.buttons input {
  max-width: 2rem;
  padding: 0.3rem;
  text-align: center;
}
.notes {
  color: red;
}
</style>
