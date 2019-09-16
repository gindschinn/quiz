<template>
    <div class="message_container">
        <div v-if="numTotal == 10">
            <p>You got {{numCorrect}} out of {{numTotal}} correct</p>
        </div>
        <div v-else>
            <p><i>Choose a category to start</i></p>
        </div>
    </div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal',
    ],
}
</script>


<style>
    .message_container {
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center
    }
    p {
        color: white;
        font-size: 28px;
    }
</style>