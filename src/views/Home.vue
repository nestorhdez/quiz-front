<template>
    <div class="home">
        <h1>Quiz game</h1>
        <Quiz :quiz="{title: '¿Esto es una pregunta?', answers: [{answer: 'esta', correct: true}, {answer: 'esta no', correct: false}]}"/>
        <button>Next</button>
    </div>
</template>

<script>

import Quiz from '../components/Quiz.vue';

export default {
    name: 'home',
    components: {
        Quiz
    }
}
</script>

<style scoped>
    h1 {
        margin-bottom: 40px;
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