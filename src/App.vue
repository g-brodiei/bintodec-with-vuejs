<template>
  <div id="app">
    <section class="container">
      <h1>Change Binary to Decimal!</h1>
      <div class="row">
        <div class="entry-section col">
          <h2>Entry Section</h2>
          <div class="row">
            <div v-if="errors.length" class="col-12">
              <div id="error" class="alert alert-danger" role="alert" v-for="(error, index) in errors" :key="index">{{error}}</div>
            </div>
            <div class="form-group col-8 offset-2">
              <label for="binary">Binary Number</label>
              <input id="binary" type="text" class="form-control" placeholder="Binary number" aria-describedby="binaryHelp" v-model="binNum">
              <small id="binaryHelp" class="form-text text-muted">fill in only 0 and 1 digits.</small>
              <button type="submit" class="btn btn-primary" @click="checkForm">Submit</button>
            </div>
          </div>
        </div>
        <div class="display-section col">
          <h2>Result Section</h2>
          <div class="">
            <span v-for="(num, index) in listDisplay" :key="index" class="number">{{num*2}}<sup>^{{index}}</sup><span v-if="index !== last"> + </span> </span>
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

      /* run method when no error occurs */
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
  },
  computed: {
    last(){
      return Object.keys(this.listDisplay).length-1;
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

.number {
  font-size: 20px;
  font-weight: bold;
  color: brown;
}

.number sup {
  font-weight: normal;
  color: blue;
}
</style>
