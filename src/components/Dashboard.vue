<template>
  <div>
    <div class="container-fluid" style="background-color: #3c8dbc; height:60px;margin-top: 0px;">
      <h2
        style="color: white;float: left; margin-top: 10px;margin-left: 20px; font-weight: 500;"
      >Farhan</h2>
      <a
        class="pull-right"
        href="#"
        style="float: right; margin-top: 20px;"
        @click="logout()"
      >Logout</a>
      <div style="clear: both;"></div>
    </div>
    <div class="row">
      <div class="col-md-2" style="background-color:#222d32; min-height: 100vh;">
        <ul style="color: white; margin-top: 50px; list-style-type: none;">
          <!-- <li>

                        <router-link to="/dashboard/home">
                            <i class="fa fa-folder" style="margin-right:5px"></i> Home
                        </router-link>
          </li>-->
          <li>
            <router-link to="/dashboard/employee">
              <i class="fa fa-folder" style="margin-right:5px"></i> Employee
              List
            </router-link>
          </li>
          <li>
            <router-link to="/dashboard/project">
              <i class="fa fa-folder" style="margin-right:5px"></i> Project
            </router-link>
          </li>
        </ul>
      </div>
      <div
        class="col-md-10"
        style="background-color:white; min-height:100vh; padding:10px; padding-left: 20px; margin-top:20px;"
      >
        <router-view />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "admin",
  data() {
    return {};
  },
  methods: {
    logout() {
      //Todo: Clear JWT Token from local storage
      localStorage.setItem('token',"");
      this.$router.push({ name: "login" });
    }
  },
  mounted() {
    if(localStorage.getItem('token')==""){
      this.$router.push({ name: "login" });
    }
  }
};
</script>
<style scoped>
a,
a:visited {
  color: lightgray;
}

li {
  margin-top: 10px;
}

a:hover {
  text-decoration: none;
}

/* a:visited{
        color: lightgray;
    } */
</style>
