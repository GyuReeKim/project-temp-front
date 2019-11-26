<template>
  <div>
    <!-- <p v-for=""></p>     -->
  </div>
</template>

<script>
export default {
  name: 'MypageForm',
  // methods: {
  //   getReviews(){
  //     return
  //     axios.get(`http://localhost:8000/api/v1/accounts/${this.user.id}/detail/`)
  //   }
  //   // 여기에 axios를 포함한 함수 만들고
  // },
  mounted: {
    // 함수
    getReviews(){
      console.log(this.user)
    }
  }

}
</script>

<style>

</style>