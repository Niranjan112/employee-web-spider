<template>
  <v-app>
    <v-container>
      <div>
        <div>
          <v-snackbar top v-model="snackbar.show" :timeout="snackbar.timeout">
            {{ snackbar.text }}

            <template v-slot:action="{ attrs }">
              <v-btn
                color="blue"
                text
                v-bind="attrs"
                @click="snackbar.show = false"
              >
                Close
              </v-btn>
            </template>
          </v-snackbar>
        </div>
      </div>

      <div>
        <v-data-table
          v-model="selectedEmployees"
          :headers="headers"
          :items="employees"
          item-key="id"
          class="elevation-1"
          show-select
          :search="search"
          :loading="loading"
        >
          <template v-slot:top>
            <v-row class="mt-5">
              <v-col>
                <v-text-field
                  v-model="search"
                  label="Search"
                  class="mx-4"
                ></v-text-field>
              </v-col>
              <v-col class="mt-3">
                <AddEmployeeComponent @addNewEmployee="addNewEmployee" />
              </v-col>
              <v-col class="mt-6">
                <v-btn color="red lighten-2" @click="deleteEmployee"
                  >Delete Employees</v-btn
                >
              </v-col>
            </v-row>
          </template>
        </v-data-table>
      </div>
    </v-container>
  </v-app>
</template>

<script>
import AddEmployeeComponent from "./components/AddEmployeeComponent";

export default {
  name: "App",

  components: {
    AddEmployeeComponent,
  },

  async mounted() {
    try {
      this.loading = true;

      const employeesResponse = await fetch(
        "https://dummy.restapiexample.com/api/v1/employees"
      );
      const employeesJsonData = await employeesResponse.json();

      if (employeesJsonData.status === "success") {
        employeesJsonData.data.forEach((employee) => {
          this.employees.push({
            id: employee.id,
            firstName: employee.employee_name.split(" ")[0],
            lastName: employee.employee_name.split(" ")[1],
            salary: employee.employee_salary,
            age: employee.employee_age,
          });
        });

        this.snackbar.show = true;
        this.snackbar.text = "Loaded Data Successfully";
        this.loading = false;
      } else {
        this.loading = false;
        this.snackbar.show = true;
        this.snackbar.text = "Failed to load data";
      }
    } catch (error) {
      this.loading = false;
      this.snackbar.show = true;
      this.snackbar.text = "Failed to load data";
    }
  },

  data: () => ({
    search: "",
    employees: [],
    selectedEmployees: [],
    loading: true,
    showDialog: false,
    snackbar: {
      show: false,
      text: "My timeout is set to 2000.",
      timeout: 2000,
    },
  }),

  computed: {
    headers() {
      return [
        { text: "First Name", value: "firstName" },
        { text: "Last Name", value: "lastName" },
        { text: "Salary", value: "salary" },
        { text: "Age", value: "age", sortable: false },
      ];
    },
  },

  methods: {
    addNewEmployee(employee) {
      employee.id = this.employees.length + 1;
      this.employees.push(employee);
    },
    deleteEmployee() {
      this.employees = this.employees.filter(
        (employee) => !this.selectedEmployees.includes(employee)
      );
    },
  },
};
</script>
