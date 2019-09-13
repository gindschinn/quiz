<template>
    <div class="message_container">
        <p v-if="numTotal == 10">
            You've got {{numCorrect}} out of {{numTotal}}
        </p>
        <p v-else>Choose a category to start</p>
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