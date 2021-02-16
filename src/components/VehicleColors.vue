<template>
  <option :value="color">{{color}}</option>
</template>

<script>
export default {
    props: {
        color: {
            type: String,
            required: true
        }
    }
}
</script>