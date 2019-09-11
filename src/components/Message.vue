<template>
    <div class="message_container">
        <p v-if="numTotal == 10">
            You've got {{numCorrect}} out of {{numTotal}}
        </p>
        <p v-else>Choose a category to begin</p>
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

</style>