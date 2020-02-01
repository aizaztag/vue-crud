<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>

    <employee-form @add:employee="addEmployee"/>
    <employee-table
            :employees="employees"
            @delete:employee="deleteEmployee"
            v-on:edit="editEmployee"
            @done="success"
    />
  </div>
</template>

<script>
  import EmployeeTable from '@/components/EmployeeTable.vue'
  import EmployeeForm from '@/components/EmployeeForm.vue'

  export default {
    name: 'app',
    components: {
      EmployeeTable,
      EmployeeForm
    },
    data() {
      return {
        employees : [],
        /*employees: [
          {
            id: 1,
            name: 'Richard Hendricksddddwdws',
            email: 'richard@piedpiper.com',
          },
          {
            id: 2,
            name: 'Bertram Gilfoyle',
            email: 'gilfoyle@piedpiper.com',
          },
          {
            id: 3,
            name: 'Dinesh Chugtai',
            email: 'dinesh@piedpiper.com',
          },
        ],*/
      }
    },
    mounted() {
      this.getEmployees()
    },


    methods: {
      /*addEmployee(emp) {
        const lastId =
                this.employees.length > 0
                        ? this.employees[this.employees.length - 1].id
                        : 0;
        const id = lastId + 1;
        const newEmployee = { ...emp, id };

        this.employees = [...this.employees, newEmployee];

      },*/
      /*deleteEmployee(id) {
        this.employees = this.employees.filter(function(employee){
          return  employee.id !== id
        });
      },
      editEmployee1(updatedEmployee) {
        console.log(updatedEmployee);
        this.employees = this.employees.map(employee =>
        employee.id === updatedEmployee.id ? updatedEmployee : employee
      )
      },*/
      success(emp){
        console.log('emp' , emp);
      },

      async getEmployees() {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/users')
      const data = await response.json()
      this.employees    = data
    } catch (error) {
      console.error(error)
    }
  },
    async addEmployee(employee) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
        })


      //  const data = await response.json()
        this.employees = [...this.employees, employee]
        console.error('this.data' , response)

      } catch (error) {
        console.error(error)
      }
    },

    async editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
        })
        const data = await response.json()
        this.employees = this.employees.map(employee => (employee.id === id ? data : employee))
      } catch (error) {
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
    },
    }//methods
  }
</script>

<style>
  @import "assets/style.css";
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 680px;
  }
</style>