<template>
  <div id="app">
    <span v-if="step < 7"><b>step{{step+1}}</b></span>
    <OyasumiHeader/>
    <InputQuestion
      v-if="step === 0"
      key="start"
      @start="handleStart"
    />

    <SelectQuestion
      key="question1"
      v-if="step === 1"
      v-model="questions.q1"
      @next="handleNext"
      :message="messageList[0]"
      :choices="choicesList[0]"
    />
    <SelectQuestion
      key="question2"
      v-if="step === 2"
      v-model="questions.q2"
      @next="handleNext"
      :message="messageList[1]"
      :choices="choicesList[1]"
    />

    <SelectQuestion
      key="question3"
      v-if="step === 3"
      v-model="questions.q3"
      @next="handleNext"
      :message="messageList[2]"
      :choices="choicesList[2]"
    />

    <SelectQuestion
      key="question4"
      v-if="step === 4"
      v-model="questions.q4"
      @next="handleNext"
      :message="messageList[3]"
      :choices="choicesList[3]"
    />

    <SelectQuestion
      key="question5"
      v-if="step === 5"
      v-model="questions.q5"
      @next="handleNext"
      :message="messageList[4]"
      :choices="choicesList[4]"
    />

    <SelectQuestion
      key="question6"
      v-if="step === 6"
      v-model="questions.q6"
      @next="handleNext"
      :message="messageList[5]"
      :choices="choicesList[5]"
    />

    <OyasumiResult
      :questions="questions"
      :step="step"
    />
  </div>
</template>

<script>
import OyasumiHeader from "./components/OyasumiHeader.vue";
import InputQuestion from "./components/InputQuestion.vue";
import SelectQuestion from "./components/SelectQuestion.vue";
import OyasumiResult from "./components/OyasumiResult.vue";

export default {
  name: "app",
  components: {
    OyasumiHeader,
    InputQuestion,
    SelectQuestion,
    OyasumiResult
  },
  data() {
    return {
      step: 0,
      questions: {
          name: "",
          title: 1,
          q1: 1,
          q2: 1,
          q3: 1,
          q4: 1,
          q5: 1,
          q6: 1
        }
      };
  },
  mounted() {
    //result画面を表示
    const params = parse(location.search.replace("?", ""));
    const isValid = [
      "name",
      "title",
      "q1",
      "q2",
      "q3",
      "q4",
      "q5",
      "q6"
    ].every(val => {
      if (!params[val]) {
        return false;
      }
      if (val != "name" && parseInt(params[val]) < 1) {
        return false;
      }
      return true;
    });
    if (isValid) {
      const questions = {
        name: params.name,
        title: parseInt(params.title),
        q1: parseInt(params.q1),
        q2: parseInt(params.q2),
        q3: parseInt(params.q3),
        q4: parseInt(params.q4),
        q5: parseInt(params.q5),
        q6: parseInt(params.q6)
      };
      this.questions = questions;
      this.step = 7;
    }
  },
  methods: {
    handleStart(startData) {
      console.log("start");
      console.log(startData);
      this.questions.name = startData.name;
      this.questions.title = startData.title;
      this.step++;
    },
    handleNext() {
      this.step++;
    }
  },
  computed: {
      messageList() {
        return [
          "では、書き出しはどうしましょうか。",
          "なるほど、どうやって切り出しますか",
          "どれくらい休みますか",
          "言いわけいれておきますか",
          "仕事はどうしますか",
          "では、しめのフレーズをどうぞ"
        ];
      },
      choicesList() {
        return [
          [
            "まじめに",
            "フレンドリーに",
            "エスプリをきかせて",
            "病気が辛そうな感じで"
          ],
          ["単刀直入に", "低姿勢に", "スイートに", "ものすごい辛そうな感じで"],
          [
            "１日",
            "１日なんだけど、半日って言っておく",
            "しばらく",
            "もうわからないぐらい"
          ],
          ["自分のせい", "会社のせい", "猫のせい", "宇宙のせい"],
          [
            "まじめなところを見せる",
            "逃げる",
            "同僚に押し付け",
            "ひらきなおり"
          ],
          ["ふつうに", "辛そうに", "ちょっと冗談を交えつつ", "イタリア人みたく"]
        ];
      }
    }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* margin-top: 60px; */
}
</style>