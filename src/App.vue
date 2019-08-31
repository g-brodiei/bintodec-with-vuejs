<template>
  <div id="app">
    <section class="container">
      <h1>Change Binary to Decimal!</h1>
      <div class="row">
        <div class="entry-section col">
          <h2>Entry Section</h2>
          <div>
            <div v-if="errors.length">
              <div id="error" class="alert alert-warning" role="alert" v-for="(error, index) in errors" :key="index">{{error}}</div>
            </div>
            <input id="binary" type="text" placeholder="Binary number" v-model="binNum">
            <button @click="checkForm">Submit</button>
          </div>
        </div>
        <div class="display-section col">
          <h2>Result Section</h2>
          <div>
            <span v-for="(num, name, index) in listDisplay" :key="index">{{num*2}}^<sup>{{name}}</sup> + </span>
            <span>=</span>
          </div>
          <p><strong>Result: {{DeciNum}}</strong></p>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      binNum: null,
      errors: [],
      DeciNum: null,
      listDisplay: [],
    }
  },
  methods: {
    checkForm: function(){
      this.errors = [];

      /* must be a number */
      if(isNaN(this.binNum)){
        this.errors.push('this must be a number');
      }

      /* can only contain 8 digits */
      if(this.binNum.length > 8){
        this.errors.push('must be under 8 digits');
      }

      /* should be only 0 and 1 */
      if(this.binNum.match(/^([0-1])+$/g) === null){
        this.errors.push('must be only 0 or 1');
      }
      if(this.errors.length == 0){
        this.binToDec();
      }
    },

    /* change binary to decimal */
    binToDec: function(){
      /* split the text into an array */
      const numList = this.binNum.split("")
      
      /* pass each value of array into parseInt to be cast as Integer */
      .map(num => parseInt(num))
      
      /* reverse the array so the index matches power's iteration */
      .reverse();
      
      this.listDisplay = numList;
      
      /* formula 
      currentValue = 1 or 0;
      sum += 2^index. 
      e.g. 1111100 binary = 2^8 + 2^7 + 2^6 + 2^5 + 2^4 + 2^3 + 2^2 + 0^1 + 0^0 = decimal 124  */

      this.DeciNum = numList.reduce(
        (accumulator, currentValue, index) => {
          return accumulator + currentValue * Math.pow(2, index);
        }
      )
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
