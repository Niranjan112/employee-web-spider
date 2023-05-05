<template>
  <v-container>
    <div class="text-center">
      <v-dialog v-model="dialog" width="500">
        <template v-slot:activator="{ on, attrs }">
          <v-btn color="green lighten-2" dark v-bind="attrs" v-on="on">
            Add New Employee
          </v-btn>
        </template>

        <v-card>
          <v-card-title class="text-h5 grey lighten-2">
            Add New Employee
          </v-card-title>

          <v-card-text>
            <v-row>
              <v-col>
                <v-text-field v-model="employee.firstName" label="First Name">
                </v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-text-field v-model="employee.lastName" label="Last Name">
                </v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-text-field v-model="employee.salary" label="Salary">
                </v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-text-field v-model="employee.age" label="Age">
                </v-text-field>
              </v-col>
            </v-row>
          </v-card-text>

          <v-divider></v-divider>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="addNewEmployee"> Submit </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
      employee: {
        firstName: "",
        lastName: "",
        salary: "",
        age: "",
      },
    };
  },
  methods: {
    addNewEmployee() {
      this.dialog = false;
      this.$emit("addNewEmployee", this.employee);
    },
  },
};
</script>
