<template>
  <div id="app">
    <el-card>
      <h3>
        Category:
        <input type="text" v-model="data.category">
      </h3>
      <table>
        <tr>
          <td>
            <h3 style="text-align:center;">Question</h3>
          </td>
          <td>
            <h3 style="text-align:center;">Answer</h3>
          </td>
        </tr>

        <tr v-for="i in data.opt" :key="i">
          <td>
            <textarea v-model="data.question[i-1]"/>&nbsp;
          </td>
          <td>
            <textarea v-on:keyup.enter="addinput(i)" v-model="data.correct_answers[i-1]"/>
          </td>
        </tr>
      </table>
      <br>
      <el-button type="success" v-on:click="add()">Add Question</el-button>&emsp;
      <el-button type="success" v-on:click="saveFile()">saveFile</el-button>&emsp;
      <el-button type="success" v-on:click="navigate()">Go to your Quiz page</el-button>
    </el-card>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      data: {
        opt: 1,
        category: "",
        type: "mtf",
        difficulty: "medium",
        question: [""],
        correct_answers: [""],
        correct: [""]
      },
      arr: []
    };
  },

  methods: {
    add() {
      for (var i = 0; i < this.data.opt; i++) {
        this.data.correct[i] = this.data.correct_answers[i];
      }
      this.arr.push({
        category: this.data.category,
        type: this.data.type,
        difficulty: this.data.difficulty,
        question: this.data.question,
        correct_answers: this.data.correct_answers,
        correct: this.data.correct
      });
      this.data.opt = 1;
      this.data.category = "";
      this.data.question = [""];
      this.data.correct_answers = [""];
      this.data.correct = [""];
      console.log(this.arr);
    },
    saveFile() {
      const data = JSON.stringify(this.arr);
      window.localStorage.setItem("arr", data);
      const obj = JSON.parse(window.localStorage.getItem("arr"));
      console.log(obj);
    },
    navigate() {
      this.$router.push("/quiz");
    },
    addinput(i) {
      if (this.data.question[i] !== "" && this.data.correct_answers[i] !== "") {
        if (this.data.opt === i) {
          this.data.opt = this.data.opt + 1;
        }
      }
    }
  }
};
</script>

