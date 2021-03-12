<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>
    <button @click="display">DisplayThis</button>

    <employee-form @add:employee="addEmployee" />
    <employee-table 
      :employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee" />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import EmployeeTable from '@/components/EmployeeTable.vue'
import EmployeeForm from '@/components/EmployeeForm.vue'



export default {
  name: 'App',
  components: {
   EmployeeTable,
   EmployeeForm,
  },
  data() {
    return {
      employees: [
        // {
        //   id:1,
        //   name:'Richard Hendricks',
        //   email:'richard@piedpier.com',
        // },
        // {
        //   id:2,
        //   name:'Bertram Gilfoyel',
        //   email: 'gilfoy@piepier.com',
        // },
        // {
        //   id:3,
        //   name:'kkm',
        //   email:'kkm@pierpeid.com',
        // }
      ]
    }
  },
  mounted(){
    this.getEmployees()
  },
  methods: {
    // addEmployee(emp) {
    //   const lastId = this.employees.length > 0? this.employees[this.employees.length - 1].id : 0;
    //   const id = lastId + 1;
    //   const newEmployee = {...emp, id};

    //   this.employees = [...this.employees, newEmployee];
    // },
    display(){
      console.log(this.employees.length);
    },
    // deleteEmployee(id){
    //   this.employees = this.employees.filter(
    //     employee => employee.id !== id
    //   )
    // },
    editEmployee(id,updateEmployee){
      this.employees = this.employees.map(employee =>
      employee.id === id ? updateEmployee : employee
      )
    },
    async getEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users')
        const data = await response.json()
        this.employees = data
      } catch (error) {
        console.error(error)
      }
  },
  async addEmployee(employee){
    try{
      const response = await fetch('https://jsonplaceholder.typicode.com/users',{
        method:'POST',
        body:JSON.stringfy(employee),
        headers:{'Content-type':'application/json; charset=UTF-8'},
      })
      const data = await response.json()
      this.employees = [...this.employees,data]
    } catch(error){
      console.error(error)
    }
  },
  async deleteEmployee(id) {
  try {
    await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
      method: "DELETE"
    });
    this.employees = this.employees.filter(employee => employee.id !== id);
  } catch (error) {
    console.error(error);
  }
}

  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}

</style>
