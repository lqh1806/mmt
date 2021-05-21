<template>
  <div class="container-app">
    <div class="container-quiz">
      <div class="header-quiz">
        <h1>Quiz Quiz</h1>
      </div>
      <div class="step-progress" :style="{ width: progress + '%' }"></div>
      <div
        class="box"
        v-for="(question, index) in questions.slice(a, b)"
        :key="index"
        v-show="quiz"
      >
        <div class="box-question">
          <h2>Câu hỏi {{ b }}/{{ questions.length }}</h2>
          <p>{{ question.question }}</p>
        </div>
        <div class="box-propositions">
          <ul>
            <li
              v-for="(proposition, index) in question.propositions"
              :key="index"
              class="li"
              @click="selectResponse(proposition, index)"
              :class="correct ? check(proposition) : ''"
            >
              {{ proposition.props }}
              <div
                class="fas fa-check"
                v-if="correct ? proposition.correct : ''"
              ></div>
              <div
                class="fas fa-times"
                v-if="correct ? !proposition.correct : ''"
              ></div>
            </li>
          </ul>
        </div>
      </div>
      <div class="box-score" v-if="score_show">
        <h2>Điểm của bạn là</h2>
        <h2>{{ score }}/{{ questions.length }}</h2>
        <div class="btn-restart">
          <button @click="restartQuiz">
            Restart <i class="fas fa-sync-alt"></i>
          </button>
        </div>
      </div>
      <div class="footer-quiz">
        <div v-if="progress < 100" class="box-button">
          <button
            @click="skipQuestion()"
            :style="next ? 'background-color: rgb(106, 128, 202)' : ''"
          >
            Bỏ qua
          </button>
          <button
            @click="nextQuestion()"
            :style="!next ? 'background-color: rgb(106, 128, 202)' : ''"
          >
            Tiếp tục
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      questions: [
        {
          question:
            'Các mạng máy tính được thiết kế và cài đặt theo quan điểm ?',
          propositions: [
            { props: 'Có cấu trúc đa tầng', correct: true },
            { props: 'Nhiều tầng' },
            { props: 'Theo lớp' },
            { props: 'Tập hợp' },
          ],
        },
        {
          question: 'Khi sử dụng mạng máy tính ta sẽ thu được các lợi ích ?',
          propositions: [
            { props: ' Chia sẻ tài nguyên' },
            { props: ' Quản lý tập trung' },
            { props: ' Tận dụng năng lực xử lý của máy tính' },
            { props: ' Tất cả đều đúng', correct: true },
          ],
        },
        {
          question: 'Đơn vị cơ bản đo tốc độ truyền dữ liệu là ?',
          propositions: [
            { props: 'Bit' },
            { props: 'Byte' },
            { props: 'Bps', correct: true },
            { props: 'Hz' },
          ],
        },
        {
          question:
            'Quá trình dữ liệu di chuyển từ máy tính này sang máy tính khác cần phải trải qua giai đoạn nào ?',
          propositions: [
            { props: 'Phân tích dữ liệu' },
            { props: 'Nén dữ liệu' },
            { props: 'Đóng gọi', correct: true },
            { props: 'Lọc dữ liệu' },
          ],
        },
        {
          question: 'Kết nối mạng sử dụng các giao thức khác nhau bằng các ?',
          propositions: [
            { props: 'Bộ chuyển tiếp' },
            {
              props: 'Cổng giao tiếp',
            },
            { props: 'SONET' },
            { props: 'Bộ định tuyến', correct: true },
          ],
        },
        {
          question: 'Cáp UTP Cat5e sử dụng đầu nối ?',
          propositions: [
            { props: 'RJ - 58' },
            { props: 'BNC' },
            { props: 'RJ - 45', correct: true },
            { props: 'ST' },
          ],
        },
        {
          question: 'Cáp UTP Cat 5 có bao nhiêu đôi cáp ?',
          propositions: [
            { props: '2' },
            { props: '4', correct: true },
            {
              props: '6',
            },
            { props: '8' },
          ],
        },
        {
          question:
            'Khẳng định nào sau đây là đúng khi nói về cấu trúc vật lý của mạng ?',
          propositions: [
            { props: 'Giao thức mạng (Protocol)' },
            {
              props: 'Hình trạng mạng (Topology)',
              correct: true,
            },
            { props: 'Phương tiện truyền' },
            { props: 'Các dịch vụ mạng' },
          ],
        },
        {
          question: 'Số hiệu cổng của giao thức truyền mail SMTP là ?',
          propositions: [
            { props: '23' },
            { props: '25', correct: true },
            { props: '21' },
            { props: '110' },
          ],
        },
        {
          question:
            'Trong số các cặp giao thức và cổng dịch vụ sau, cặp nào sai ?',
          propositions: [
            { props: 'SMTP:TCP Port 25' },
            { props: 'FTP:UDP Port22', correct: true },
            { props: 'HTTP:TCP Port 70' },
            { props: 'DNS:UDP Port 53' },
          ],
        },
      ],
      a: 0,
      b: 1,
      next: true,
      score_show: false,
      quiz: true,
      score: 0,
      correct: false,
      progress: 0,
    };
  },
  name: 'App',
  components: {
    //HelloWorld
  },
  computed: {},
  methods: {
    selectResponse(e) {
      this.correct = true;
      this.next = false;
      if (e.correct) {
        this.score++;
      }
    },
    check(status) {
      if (status.correct) {
        return 'correct';
      } else {
        return 'incorrect';
      }
    },
    nextQuestion() {
      if (this.next) {
        return;
      }
      this.progress = this.progress + 100 / this.questions.length;
      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      } else {
        this.a++;
        this.b++;
        this.correct = false;
        this.next = true;
      }
    },
    skipQuestion() {
      if (!this.next) {
        return;
      }
      this.progress = this.progress + 100 / this.questions.length;

      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      } else {
        this.a++;
        this.b++;
      }
    },

    restartQuiz() {
      Object.assign(this.$data, this.$options.data()); // reset data in vue
    },
  },
};
</script>

<style scoped>
* {
  font-family: 'Poppins', sans-serif;
  margin: 0;
  letter-spacing: 2px;
}

.container-app {
  display: flex;
  width: 100%;
  height: 100%;
  justify-content: center;
}

.container-quiz {
  display: flex;
  width: 40%;
  height: 70%;
  /* background-color: white; */
  text-align: center;
  flex-flow: column;
  border: 1px solid #e7eae0;
  border-radius: 10px;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
}

.header-quiz {
  display: flex;
  width: 100%;
  height: 20%;
  border-bottom: 1px solid #e7eae0;
  justify-content: center;
  align-items: center;
  background-color: #e6e6e6;
  border-radius: 10px 10px 0px 0px;
  color: rgb(0, 119, 10);
}

.container-quiz .box {
  display: flex;
  width: 100%;
  height: 70%;
  flex-flow: column;
  margin: auto;
}

.box-question {
  margin-top: 20px;
}

.box-question p {
  margin-top: 20px;
}

.box-propositions {
  margin: auto;
  display: flex;
  width: 100%;
  justify-content: center;
  align-items: center;
  font-size: 18px;
}

ul {
  display: flex;
  width: 80%;
  margin: 0;
  padding: 0;
  flex-flow: column;
}

li {
  list-style: none;
  line-height: 2;
  border: 1px solid #cdd2d2;
  margin-bottom: 20px;
  border-radius: 15px;
  cursor: pointer;
  transition: 0.3s;
}

li:hover {
  /*transform: scale(1.1);*/
  background-color: #e7eae0;
}

li > div {
  float: right;
  margin-top: 7px;
  margin-right: 7px;
  color: white;
}

.check {
  color: rgb(74, 219, 74);
}

.close {
  color: rgb(240, 117, 100);
}

.footer-quiz {
  display: flex;
  width: 100%;
  height: 10%;
  justify-content: center;
  border-top: 1px solid #e7eae0;
  background-color: #e7eae0;
  border-radius: 0px 0px 10px 10px;
}

.box-button {
  display: flex;
  width: 100%;
}

.footer-quiz .box-button button {
  width: 150px;
  height: 35px;
  outline: none;
  border: 0;
  color: white;
  font-size: 18px;
  cursor: pointer;
  border-radius: 15px;
  margin: auto;
  margin-bottom: 10px;
  letter-spacing: 2px;
  background-color: #a09f9ff5;
}

li.correct {
  border: 1px solid rgb(74, 219, 74);
  background-color: rgb(74, 219, 74);
  color: white;
  font-weight: 600;
}

li.incorrect {
  border: 1px solid rgb(240, 117, 100);
  background-color: rgb(240, 117, 100);
  color: white;
  font-weight: 600;
}

.box-score {
  display: flex;
  width: 100%;
  height: 70%;
  flex-flow: column;
}

.box-score h2 {
  margin-top: 40px;
}

i {
  display: none;
  color: white;
}

.step-progress {
  display: flex;
  width: 100%;
  height: 5px;
  background-color: rgb(106, 128, 202);
  transition: 0.5s;
}

.btn-restart {
  display: flex;
  width: 100%;
  height: auto;
  justify-content: center;
  margin-top: 50px;
}

.btn-restart button {
  width: 150px;
  height: 35px;
  outline: none;
  border: 0;
  background-color: rgb(106, 128, 202);
  color: white;
  font-size: 18px;
  cursor: pointer;
  border-radius: 15px;
  margin: auto;
  margin-bottom: 10px;
  letter-spacing: 2px;
}

.next {
  background-color: rgb(106, 128, 202);
}

@media screen and (max-width: 900px) {
  .container-quiz {
    width: 60%;
  }
}

@media screen and (max-width: 720px) {
  .container-quiz {
    width: 80%;
  }
  .footer-quiz .box-button button {
    width: 100px;
  }
}
</style>
