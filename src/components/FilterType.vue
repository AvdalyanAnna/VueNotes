<template>
  <select @change="changeType(selectValue)" v-model="selectValue">
    <option>All</option>
    <option v-for="(type,index) in types" :key="index"> {{ type }}</option>
  </select>
</template>

<script>
export default {
  name: "FilterType",
  props: {
    types: {
      type: Array,
      required: true,
    }
  },
  data(){
    return{
      selectValue:"All"
    }
  },
  methods:{
    changeType(selectValue){
      this.$emit('changeType', selectValue)
    }
  }
}
</script>

<style scoped>
select{
  border-radius: 30px;
  padding: 5px 10px;
}
</style>