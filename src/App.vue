<template>
  <div id="app">
    <div class="container">
      <span class="title">1, 物种丰富度</span>
      <el-input
        class="input"
        placeholder="依次输入S(样方内物种数目)和N(样方内物种个数之和)，用空格隔开"
        v-model="input1"
        clearable>
      </el-input>
      <el-button class="btn" type="success" @click="formula1()">提交</el-button>
      <span class="result">结果： {{this.result1}} </span>
    </div>
    <div class="container">
      <span class="title">2，辛普森优势度指数</span>
      <el-input
        class="input"
        placeholder="依次输入每个种的个体数，用空格隔开"
        v-model="input2"
        clearable>
      </el-input>
      <el-button class="btn" type="success" @click="formula2()">提交</el-button>
      <span class="result">结果:  {{this.result2}}</span>
    </div>
    <div class="container">
      <span class="title">3, S-W 指数</span>
      <el-input
        class="input"
        placeholder="依次输入每个种的个体数，用空格隔开"
        v-model="input4"
        clearable>
      </el-input>
      <el-button class="btn" type="success" @click="formula4()">提交</el-button>
      <span class="result">结果:  {{this.result4}}</span>
    </div>
    <div class="container">
      <span class="title">4，均匀度指数</span>
      <el-input
        class="input"
        placeholder="依次输入每个种的个体数，用空格隔开"
        v-model="input3"
        clearable>
      </el-input>
      <el-button class="btn" type="success" @click="formula3()">提交</el-button>
      <span class="result">结果:  {{this.result3}}</span>
    </div>
    <div class="container2">
      <span class="title">5, 一次得到所有结果</span>
      <el-input
        class="input5"
        placeholder="依次输入每个种的个体数，用空格隔开"
        v-model="input5"
        clearable>
      </el-input>
      <el-button class="btn" type="success" @click="formula5()">提交</el-button>
      <span class="result">结果: </span>
      <el-table
      v-if="this.result5"
      :data="tableData"
      style="width: 100%">
      <el-table-column
        prop="name"
        label="名称"
        width="180">
      </el-table-column>
      <el-table-column
        prop="result"
        label="结果"
        width="180">
      </el-table-column>
    </el-table>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      input1: '',
      result1: '',
      input2: '',
      result2: '',
      input3: '',
      result3: '',
      input4: '',
      result4: '',
      input5: '',
      result5: '',
      tableData: [{
        result: '',
        name: '',
      }, {
        result: '',
        name: '',
      }, {
        result: '',
        name: '',
      }, {
        result: '',
        name: '',
      }]
    }
  },
  methods: {
    formula1() {
      const arrg = this.input1.split(' ').map(item => +item);
      if(arrg.length < 2 || arrg.length !== 2 || arrg.some(item => isNaN(item))) {
        this.$message.error('数据输入有误');
        return;
      }
      const func1 = (s, x) => (s - 1 / Math.log(x)).toFixed(4)
      const result = func1(arrg[0], arrg[1])
      this.result1 = result;
    },

    formula2() {
      const arrg = this.input2.split(' ').map(item => +item);
      if(arrg.length < 2 || arrg.some(item => isNaN(item))) {
        this.$message.error('数据输入有误');
        return;
      }
      const N = arrg.reduce((prev, cur) => prev + cur);
      const pn = arrg.map(item => +Math.pow((item / N).toFixed(4), 2).toFixed(4));
      const result = 1 - pn.reduce((prev, cur) => prev + cur)
      this.result2 = result;
    },

    formula3() {
      const arrg = this.input3.split(' ').map(item => +item);
      if(arrg.length < 2 || arrg.some(item => isNaN(item))) {
        this.$message.error('数据输入有误');
        return;
      }
      const S = arrg.length;
      const N = arrg.reduce((prev, cur) => prev + cur);
      const pn = arrg.map(item => +((item / N) * Math.log(item / N)).toFixed(4));
      const result = (-pn.reduce((prev, cur) => prev + cur) / Math.log(S)).toFixed(4)
      this.result3 = result;
    },

    formula4() {
      const arrg = this.input4.split(' ').map(item => +item);
      if(arrg.length < 2 || arrg.some(item => isNaN(item))) {
        this.$message.error('数据输入有误');
        return;
      }
      const N = arrg.reduce((prev, cur) => prev + cur);
      const pn = arrg.map(item => +((item / N) * Math.log(item / N)).toFixed(4));
      const result = (-pn.reduce((prev, cur) => prev + cur)).toFixed(4)
      this.result4 = result;
    },

    formula5() {
      let result = '';
      const arrg = this.input5.split(' ').map(item => +item);
      if(arrg.length < 2 || arrg.some(item => isNaN(item))) {
        this.$message.error('数据输入有误');
        return;
      }
      const S = arrg.length;
      const N = arrg.reduce((prev, cur) => prev + cur);
      const func1 = (s, x) => (s - 1 / Math.log(x)).toFixed(4);
      const result1 = func1(S, N);
      result += result1;

      const pn1 = arrg.map(item => +Math.pow((item / N).toFixed(4), 2).toFixed(4));
      const result2 = 1 - pn1.reduce((prev, cur) => prev + cur)
      result = result + ' ' + result2;

      const pn2 = arrg.map(item => +((item / N) * Math.log(item / N)).toFixed(4));
      const result3 = (-pn2.reduce((prev, cur) => prev + cur)).toFixed(4)
      result = result + ' ' + result3;

      const result4 = (result3 / Math.log(S)).toFixed(4);
      result = result + ' ' + result4;

      this.result5 = result;
      this.tableData[0].name = '物种丰富度';
      this.tableData[0].result = result1;

      this.tableData[1].name = '辛普森优势度指数';
      this.tableData[1].result = result2;

      this.tableData[2].name = 'S-W 指数';
      this.tableData[2].result = result3;

      this.tableData[3].name = '均匀度指数';
      this.tableData[3].result = result4;
    }

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  width: 80%;
  height: 200px;
  border: 1px solid #cbcbcb;
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  padding-left: 20px;
}

.container2 {
  width: 80%;
  border: 1px solid #cbcbcb;
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  padding-left: 20px;
}

.container2 .title {
  color: #5A9CF8;
  margin-top: 10px;
  margin-bottom: 15px
}

.container2 .input {
  width: 80%;
}

.container2 .btn {
  color: #fff;
  width: 100px;
  margin-top: 15px;
  text-align: center;
  margin-bottom: 15px;
}

.container .title {
  color: #5A9CF8;
}

.container .input {
  width: 80%;
}

.container .btn {
  color: #fff;
  width: 100px;
  text-align: center;
}
</style>
