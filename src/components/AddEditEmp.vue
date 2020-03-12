<template>
  <div>
    <!-- ADDmodal -->
    <!-- Modal -->
    <div id="addEmp" class="modal fade" role="dialog">
      <div class="modal-dialog">
        <!-- Modal content-->
        <div class="modal-content">
          <div class="modal-header" style="background-color: #ddd">
            <h4 class="modal-title float-left">{{modalheading1}}</h4>
            <button type="button" class="close float-right" data-dismiss="modal">&times;</button>
          </div>
          <div class="modal-body" style="padding-left:20px; padding-right:20px;">
            <form id="empfrm">
              <div class="form-group">
                <label for="empid">EmpID</label>
                <input class="form-control" v-model="emp.empid" type="text" name="empid" id />
              </div>
              <div class="form-group">
                <label for="firstname">Firstname</label>
                <input class="form-control" v-model="emp.firstname" type="text" name="firstname" id />
              </div>
              <div class="form-group">
                <label for="lastname">LastName</label>
                <input class="form-control" v-model="emp.lastname" type="text" name="lastname" id />
              </div>
              <div class="form-group">
                <label for="jobtitle">JobTitle</label>
                <select
                  type="select"
                  class="form-control"
                  v-model="emp.jobtitle"
                  name="jobtitle"
                  id
                >
                  <option v-for="jt in jobtitles" :value="jt.listid">{{jt.str1}}</option>
                </select>
              </div>
              <div class="form-group">
                <label for="registration">Registration</label>
                <select
                  type="select"
                  class="form-control"
                  v-model="emp.registration"
                  name="registration"
                  id
                >
                  <option v-for="reg in registration" :value="reg.listid">{{reg.str1}}</option>
                </select>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              @click="save()"
              type="button"
              class="btn btn-primary"
              data-dismiss="modal"
              style="width:100px"
            >Save</button>
            <!-- <button type="button" class="btn btn-default" data-dismiss="modal">Close</button> -->
          </div>
        </div>
      </div>
    </div>

    <!-- End editmodal -->
  </div>
</template>


<script>
export default {
  name: "AddEditEmp",
  // props: ["modalheading", "addMode"],
  data() {
    return {
      empID: 0,
      addMode1: true,
      modalheading1: "Add Employee",
      emp: {
        empid: 0,
        firstname: "",
        lastname: "",
        jobtitle: 0,
        registration: 0
      },
      jobtitles: [],
      registration: []
    };
  },
  methods: {
    loadeditdata(empid) {
      // this.loadJobTitle();
      this.empID = this.empid;
      this.addMode1 = false;
      //  this.modalheading1 = "Edit Employee";
      this.$axios
        .get("http://localhost:8000/api/employee/" + empid)
        .then(response => {
          this.emp = response.data;
          this.addMode1 = false;
          this.modalheading1 = "Edit Employee";
          $("#addEmp").modal("show");
        });
    },
    loadJobTitle() {
      this.$axios.get("http://localhost:8000/api/jobtitle").then(response => {
        this.jobtitles = response.data;
        console.log(response.data);
      });
    },
    loadRegistration() {
      this.$axios
        .get("http://localhost:8000/api/registration")
        .then(response => {
          this.registration = response.data;
          console.log(response.data);
        });
    },
    save() {
      var frm = $("#empfrm");
      if (this.addMode1 == true) {
        this.$axios
          // .post("http://localhost:8000/api/employee/", frm.serialize())//Data send by Form
          .post("http://localhost:8000/api/employee/", this.emp) //Data send by JSON Object
          .then(response => {
            // this.loadTable();
            this.$emit('saved',true);
          });
      } else {
        this.$axios
          // .post("http://localhost:8000/api/employee/"+ this.emp.empid, frm.serialize())
          .post(
            "http://localhost:8000/api/employee/" + this.emp.empid,
            this.emp
          )
          .then(response => {
            // this.loadTable();
            this.addMode1 = true;
            this.modalheading1 = "Add Employee";
             this.$emit('saved',true);
          });
      }
    },
    resetModal() {
      //this.loadJobTitle();
      this.modalheading1 = "Add Employee";
      this.emp.empid = 0;
      this.emp.firstname = "";
      this.emp.lastname = "";
      this.emp.jobtitle = 0;
      this.emp.registration = 0;
    }
  },
  mounted() {
    // this.loadTable();
    this.loadJobTitle();
    this.loadRegistration();
  }
};
</script>





<style>
</style>