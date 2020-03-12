<template>
  <div>
    <h1>Projects</h1>
  </div>
</template>


<script>

export default{
  mounted() {
    if(localStorage.getItem('token')==""){
      this.$router.push({ name: "login" });
    }
  }
}

</script>