<template>
    <option :value="time" >{{time}}</option>
</template>

<script>
export default {
    props: {
        time: {
            type: String,
            required: true
        }
    }
}
</script>
