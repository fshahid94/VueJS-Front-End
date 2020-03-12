<template>
  <div>
    <!-- <h1>Login</h1>
    <button type="button" @click="gotoDashboard()">CLick</button>     -->
    <!-- <h1 v-if="test">{{ user.username }}</h1> -->
    <div
      class="container"
      style="width: 400px; border: 1px solid grey; padding-top: 10px;padding-bottom: 10px;"
    >
      <h1>Login</h1>
      <form id="loginform">
        <div class="form-group">
          <label for="Username">Email</label>
          <input
            v-model="user.email"
            class="form-control"
            type="text"
            name="email"
            id="email"
          />
        </div>
        <div class="form-group">
          <label for="Password">Password</label>
          <input
            v-model="user.password"
            class="form-control"
            type="text"
            name="password"
            id="password"
          />
        </div>
        <button
          @click="signin()"
          type="button"
          class="btn btn-primary pull-right"
          style="width: 100px;"
        >
          Submit
        </button>
      </form>
    </div>
  </div>
</template>

<script>
//   export default{
//     methods:{
//       gotoDashboard(){
//         this.$router.push({name: "dashboard"});
//       }
//     }
//   }
export default {
  name: "Login",
  data() {
    return {
      // msg: 'Welcome to Your Vue.js App'
      user: {
        email: "",
        password: ""
      },
      test: true
    };
  },
  methods: {
    // signin: function() {
    //   if (this.user.username == "") {
    //     // alert("Logged In");
    //     //Todo: Save JWT Token to the Local Storage
    //     this.$router.push({ name: "dashboard" });
    //   } else {
    //     alert("Failed");
    //   }
    // }
    
    signin(){
      var frm = $("#loginform");
      this.$axios.post("http://localhost:8000/api/login", frm.serialize()).then(response => {
          //this.user = response.data;
         // console.log(response.data.access_token);

if(response.data.access_token!=null){
  localStorage.setItem('token',response.data.access_token);
  this.$router.push({ name: "dashboard" });

}else{
  alert("Wrong Input");
}
           

        })
        .catch((err)=>{
          console.log(err);
        });
    }
  }
};
</script>
