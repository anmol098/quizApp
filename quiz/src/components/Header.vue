<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled>Quiz APP</b-nav-item>
            <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal}}</b-nav-item>
        </b-nav>
    </div>
</template>
<script>
    export default {
        props: [
            'numCorrect',
            'numTotal'
        ]
    }
</script>
