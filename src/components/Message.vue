\<template>
  <div class="message">
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  name: "Message",
  props: {
    message: {
      default: null,
      type: String,
      required:true,
    },
  }

}
</script>

<style lang="scss" scoped>
.message{
  text-align: center;
  padding: 20px;
}
p{
  color:#ff0000;
}
</style>