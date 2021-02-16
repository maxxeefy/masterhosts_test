<template>
  <div class="properties">
      <p><span class="propertiesName">{{propertie.name}}:</span> <span class="propertiesValue">{{propertie.value}}</span></p>
  </div>
</template>

<script>
export default {
    props: {
        propertie: {
            type: Object,
            required: true
        }
    },
}
</script>

<style>
    .properties p {
        display: flex;
        justify-content: space-between;
        width: 80%;
        margin-left: 40px;
        margin-bottom: 10px;
    }
    .propertiesName {
        text-align: left;
    }

    .propertiesValue {
        width: 60%;
    }
</style>>
