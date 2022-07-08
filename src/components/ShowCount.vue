<template>
    {{store.state.count+1}}
</template>

<script>
    export default {
        name: "ShowCount",
        inject: ['store'],
    }
</script>