<template>
    <div id="quiz">
        <h2 id="question">{{quiz.title}}</h2>
        <div id="answers-container">
            <button :id="i" @click="check" v-for="(ans, i) in quiz.answers" :key="i">{{ans.answer}}</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'quiz',
    props: {
        quiz: {
            title: String,
            answers: [Object]
        }
    },
    methods: {
        check(e) {
            const index = e.target.id;
            this.quiz.answers[index].correct ? e.target.classList.add('correct') : e.target.classList.add('incorrect');
        }
    }
}
</script>

<style scoped>
    #quiz {
        width: 80%;
        margin: 0 auto;
        padding: 20px;
        box-shadow: 0 0 4px 1px rgba(0, 0, 0, .3);
    }

    #question {
        margin-bottom: 20px;
        font-size: 1.4rem;
        text-align: justify;
    }

    #answers-container {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
    }

    button {
        flex-basis: 100%;
        margin: 10px;
        padding: 10px;
        border: 1px solid rgba(0, 0, 0, .3);
        border-radius: 4px;
        background: none;
        font-size: 1rem;
        font-weight: 500;
        transition-duration: .2s;
    }

    .correct, .incorrect {
        color: white;
    }

    .correct {
        background-color: #42b983;
    }

    .incorrect {
        background-color: #c54040;
    }

    @media(min-width: 678px) {

        #question {
            width: 80%;
            margin: 0 auto 20px;
        }

        button {
            flex-basis: 40%;
        }
    }

</style>