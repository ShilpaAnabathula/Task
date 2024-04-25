<template>
  <v-app :class="{'dark-theme':darkTheme}">
    <v-card>
      <v-layout>
        <!-- Navigation Drawer -->
        <v-navigation-drawer
          v-model="drawer"
          app
          permanent
          style="width: 100%; max-width: 186px; gap: 0px; opacity: 0px; border-radius: 0px 25px 25px 0px; opacity: 0px; background: #702F61;"
          theme="dark"
        >
          <v-list nav class="icon">
            <v-list-item prepend-icon="mdi-view-dashboard" title="" value=""></v-list-item><span>Dashboard</span>
            <v-list-item prepend-icon="mdi-cube" title="" value=""></v-list-item><span>Product & Modules</span>
            <v-list-item prepend-icon="mdi-account" title="" value=""></v-list-item><span>User</span>
            <v-list-item prepend-icon="mdi-account-box" title="" value=""></v-list-item><span>Registrations</span>
            <v-list-item prepend-icon="mdi-receipt" title="" value=""></v-list-item><span>Invoice</span>
            <v-list-item prepend-icon="mdi-credit-card" title="" value=""></v-list-item><span>Payments</span>
            <v-list-item prepend-icon="mdi-thumb-up" title="" value=""></v-list-item><span>Approve</span>
          </v-list>
        </v-navigation-drawer>
        
        <!-- Main Content Area -->
        <v-main>
          <!-- Employee Master Title -->
          <div>
            <h2>Employee Master <span @click="toggleDarkMode">Switch Themes<i class="fa-solid fa-circle-half-stroke" id="theme"></i></span></h2>
          </div>         
          <v-row>
            <!-- Employee Info Form -->
            <v-col cols="12" md="6" class="card">
              <v-card>
                <form @submit.prevent="saveEmployee">
                  <div class="form-control">
                    <label>Employee Info <i class="fa-solid fa-pen"></i><span v-if="!editMode">EDIT</span></label>
                  </div>
                  <div class="form-control">
                    <input v-model="employee.displayName" type="text" placeholder="Display Name">
                  </div>
                  <div class="form-control">
                    <input v-model="employee.loginID" type="text" placeholder="Login ID">
                  </div>
                  <div class="form-control">
                    <input v-model="employee.password" type="password" placeholder="Password">
                  </div>
                  <div class="form-control" v-if="!editMode">
                    <label class="sts">Status <i class="fa-solid fa-toggle-on"></i><span>Active</span></label>
                  </div>
                  <button v-if="!editMode" class="btn" type="submit"><span class="text">Save</span></button>
                  <button v-if="editMode" class="btn" type="submit"><span class="text">Update</span></button>
                </form>
              </v-card>
            </v-col>           
            <v-col cols="12" md="6" class="viewEmp">
              <v-card class="view-employee-card">
                <form class="view-employees-form" @submit.prevent="searchEmployees">
                  <label>View Employees ({{ filteredEmployees.length }})<button type="submit"></button></label>
                  <!-- <v-btn icon type="submit"> -->
                    <!-- <v-icon class="">mdi-magnify</v-icon> -->
                  <!-- </v-btn> -->
                  <input class="search-input" v-model="searchQuery" type="text" placeholder="Search" style="text-align: center;">
                </form>
                <v-divider></v-divider>
                <!-- Employee List -->
                <v-list>
                  <v-list-item v-for="(emp, index) in filteredEmployees" :key="index">
                    <v-list-item-content>
                      <div class="employee-info">
                        <img :src="emp.image" alt="Employee Image" class="img">
                        <span class="employee-name">{{ emp.displayName }}</span>
                        <v-btn class="custom-btn-class" @click="viewEmployee(index)" text>View</v-btn>
                      </div>
                    </v-list-item-content>
                  </v-list-item>
                </v-list>
              </v-card>
            </v-col>
          </v-row>
        </v-main>
      </v-layout>
    </v-card>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: true,
      editMode: false,
      editIndex: null,
      // darkTheme: localStorage.getItem('darkTheme') === 'true' || false,
        employee: {
        displayName: '',
        loginID: '',
        password: '',
        image: 'https://i.pinimg.com/474x/49/74/0f/49740fef6224d734de954f254085f332.jpg'
      },
      employees: [],
      searchQuery: '',
    };
  },
  // mounted() {
  //   const themeToggle = document.getElementById("theme");
  //   themeToggle.addEventListener('click', () => {
  //     this.toggleDarkMode();
  //   });
  // },
  methods: {
    // toggleDarkMode() {
    //   this.darkTheme = !this.darkTheme;
    //   localStorage.setItem('darkTheme', this.darkTheme);
    // },
    saveEmployee() {
      if (this.editMode) {
        // Update existing employee
        this.employees[this.editIndex] = { ...this.employee };
        this.editMode = false;
      } else {
        // Add new employee
        this.employees.push({ ...this.employee });
      }
      this.clearForm();
    },
    clearForm() {
      this.employee = { displayName: '', loginID: '', password: '', image: 'https://i.pinimg.com/474x/49/74/0f/49740fef6224d734de954f254085f332.jpg' };
    },
    searchEmployees() {
      // No need for implementation in this example
      // Search is handled in computed property
    },
  },
  computed: {
    filteredEmployees() {
      return this.employees.filter(emp => {
        return emp.displayName.toLowerCase().includes(this.searchQuery.toLowerCase()) || emp.loginID.toLowerCase().includes(this.searchQuery.toLowerCase());
      });
    }
  },
};
</script>

<style scoped>
.form-control {
  margin-bottom: 25px;
  margin-top: 20px;
  margin-left: 30px;
}
.form-control label {
  display: block;
  margin-bottom: 10px;
  font-size: 20px;
  font-weight: bold;
}
.card {
  width: 100%; 
  height: 654px;
  margin-top: 20px;
  border-radius: 29px 0 0 0;
  background: #FFFFFF;
}
.btn {
  background: #702F61;
  border: none;
  border-radius: 35px;
  cursor: pointer;
  margin-left: 140px;
  margin-top: 10px;
  margin-bottom: 40px;
  width: 190px;
  height: 60px;
}
.text {
  font-family: Roboto;
  font-size: 18px;
  font-weight: 700;
  line-height: 25.78px;
  letter-spacing: 0.03em;
  text-align: left;
  color: #FFFFFF;
}
.icon {
  margin-left: 55px;
  margin-top: 80px;
}
.icon span {
  height: 19px;
  width: 78px;
  line-height: 18.75px;
  font-family: Roboto;
  font-size: 16px;
  font-weight: 400;
  color: #FFFFFF;
}
h2 {
  margin-top: 42px;
  line-height: 41.02px;
  letter-spacing: 0.03em;
  text-align: left;
}
h2 span {
  margin-left: 580px;
  font-size: 16px;
}
.form-control input[type="text"],
.form-control input[type="password"] {
  border: 1px solid #ccc;
  display: block;
  width: 93%;
  padding: 10px;
}
.sts i.fa-solid.fa-toggle-on {
  color: #702F61;
  margin-left: 270px;
}
.form-control label i.fa-solid.fa-pen {
color: #702F61;
margin-left: 210px;
height: 24px;
width: 24px;
}
.form-control label span {
  font-family: Roboto;
  font-size: 20px;
  font-weight: 700;
  line-height: 12px;
}
.viewEmp{
  width: 100%;
  height: 474px;
  border-radius: 29px 0 0 0;
  margin-top: 20px;
  display: flex;
  align-items: stretch;
}
.view-employee-card {
  flex: 1;
}
.view-employee-list {
  height: 100%; 
  overflow-y: auto; 
}
.img{
  width: 62.39px;
  height: 58px;
  top: 357px;
  left: 902px;
  gap: 0px;
  opacity: 0px;
  border-radius: 50%;
  object-fit: cover;
}
.v-list-item-content {
  padding-left: 12px;
}
.employee-info {
  display: flex;
  align-items: center;
}
.employee-name {
  width: 112.95px;
  height: 22px;
  top: 375px;
  left: 993.43px;
  gap: 0px;
  opacity: 0px;
  font-family: Roboto;
  font-size: 19px;
  font-weight: 700;
  line-height: 22.27px;
  letter-spacing: 0.03em;
  text-align: left;
  margin-left: 20px;
  color: #313333;
}
.custom-btn-class {
  color: #FFFFFF;
  margin-left: 70px;
  width: 167.81px;
  height: 47px;
  background: #702F61;
  border-radius: 9px;
}
.view-employees-form label{
  margin-left: 20px;
  font-size: 20px;
  font-weight: 700;
  color: #4A4A4A;
}
.search-input{
  margin-left: 100px;
  width: 151px;
  height: 36px;
  border-radius: 8px;
  border: 1px solid #4A4A4A
}
/* ..... */


/* Responsive adjustments */
@media screen and (max-width: 768px) {
  .form-control {
    margin-left: 0; 
  }
  .btn {
    margin-left: 0; 
    width: 100%; 
  }
  h2 span {
    margin-left: 0; 
  }
  .search-input {
    margin-left: 0; 
    width: 100%; 
  }
}
</style>
