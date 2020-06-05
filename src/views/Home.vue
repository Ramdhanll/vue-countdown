<template>
  <div>
    <Counter 
      :year="2020"
      :month="5"
      :date="5"
      :hour="24"
      :minutes="50"
      :second="10"
      :milisecond="0"/>
  </div>
</template>

<script>
import Counter from '../components/Counter'
export default {
  components : {
    Counter
  }
}
</script>

<style>

</style>