<template>
    <div class="home">
        <h1>Quiz game</h1>
        <span v-if="error.status">{{error.msg}}</span>
        <span v-if="end" id="end">THE END</span>
        <Quiz :quiz="questions[questionIndex]" v-if="questions.length > 0"/>
        <button @click="next" v-if="!error.status && questions.length > 0">Next</button>
    </div>
</template>

<script>

import Quiz from '../components/Quiz.vue';
import Axios from '../interceptor';

export default {
    name: 'home',
    data() {
        return {
            questions: [],
            questionIndex: 0,
            error: {
                status: false,
                msg: ''
            },
            end: false
        }
    },
    methods: {
        getQuestions() {
            this.error.status = false;
            Axios.get(`${this.$url}/questions`)
                .then(res => {
                    if(res.data.res.length === 0) {
                        this.error.status = true;
                        this.error.msg = 'There are no questions to play...';
                    }
                    this.questions = res.data.res;
                })
                .catch(() => {
                    this.error.status = true;
                    this.error.msg = 'Something wrong happend...';
                });
        },
        setQuestion() {
            this.questionIndex = Math.floor((Math.random() * this.questions.length));
        },
        removeQuestion() {
            this.questions = this.questions.filter((ques, i) => i != this.questionIndex);
        },
        next() {
            this.removeQuestion();
            this.setQuestion();
            if(this.questions.length === 0) {
                this.end = true;
            }
        }
    },
    created() {
        this.getQuestions();
        this.setQuestion();
    },
    components: {
        Quiz
    }
}
</script>

<style scoped>
    h1 {
        margin-bottom: 40px;
    }

    span {
		color: #c54040;
        font-weight: 500;
        font-size: 1.2rem;
        margin-bottom: 20px;
        display: inline-block;
	}

    button {
        background-color: #2c3e50;
        padding: 10px 20px;
        border: 1px solid #f9f8f8;
        color: white;
        font-size: .9rem;
        font-weight: 500;
        border-radius: 4px;
        margin-top: 40px;
        transition-duration: .2s;
    }

    button:active {
        color: #2c3e50;
        background-color: white;
    }

</style>