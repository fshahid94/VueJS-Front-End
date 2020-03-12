<template>
  <div class="container" style="border: 1px solid #3c8dbc; padding-left:8px; padding-right:8px">
    <div style="margin-top:10px;">
      <button
        type="button"
        class="btn btn-default btn-md float-right"
        data-toggle="modal"
        data-target="#addEmp"
        style="color:#1A6888 !important;  border-color: #ccc;"
        @click="resetModal()"
      >
        <i class="fa fa-plus"></i>
        Add Employee
      </button>

      <h1 style="font-size: 28px">Employee Table</h1>
    </div>

    <!-- <AddEditEmp ref="editmodalcomponent" :modalheading="modalheading" :addMode="addMode"/> -->
    <AddEditEmp v-on:saved="loadTable()" ref="editmodalcomponent" />

    <table
      class="table table-striped table-bordered"
      style="border:1px solid #cecece !important"
      id="tbl"
    >
      <thead>
        <tr>
          <th>EmpID</th>
          <th>FirstName</th>
          <th>LastName</th>
          <th>JobTitle</th>
          <th>Registration</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="emp in emps">
          <td>{{emp.empid}}</td>
          <td>{{emp.firstname}}</td>
          <td>{{emp.lastname}}</td>
          <td>{{emp.jobtitle}}</td>
          <td>{{emp.registration}}</td>
          <td>
            <a @click="showempeditmodal(emp.empid)" href="#">Edit</a>
            |
            <a @click="remove(emp.empid)" href="#">Delete</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>


<script>
import AddEditEmp from "./AddEditEmp.vue";
export default {
  components: {
    AddEditEmp
  },
  data() {
    return {
      emps: [],
      modalheading: "Add Employee",
      addMode: true,
      empid: 0
    };
  },
  methods: {
    resetModal() {
      this.$refs.editmodalcomponent.resetModal();
    },
    loadTable() {
//       let config = {
//   headers: {
//     header1: value,
//   }
// }

// let data = {
//   'HTTP_CONTENT_LANGUAGE': self.language
// }

// axios.post(URL, data, config).then(...)
let config = {
  headers: {
    Accept: "application/json",
    Authorization: "Bearer "+ localStorage.getItem('token'),
  }
}
      this.$axios.get("http://localhost:8000/api/employee",config).then(response => {
        this.emps = response.data;
        console.log(this.emps);
      });
    },

    remove(empid) {
      this.$axios
        .delete("http://localhost:8000/api/employee/" + empid)
        .then(response => {
          this.loadTable();
        });
    },

    showempeditmodal(empid) {
      this.$refs.editmodalcomponent.loadeditdata(empid);
    }
  },
  mounted() {
    if (localStorage.getItem("token") == "") {
      this.$router.push({ name: "login" });
    }else{
      this.loadTable();
    }
    
  }
};
</script>

<style scoped>
.form-group label {
  margin-bottom: 0px;
}

#tbl tr {
  line-height: 0.7;
}

#tbl th {
  background-color: #2a88bd;
  color: #fff;
}

#tbl {
  border-bottom: 1px solid blue !important;
}
</style>