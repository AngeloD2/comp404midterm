<template>
    <v-form
      class="pa-6"
    >
      <v-text-field
        v-model="groupname"
        :counter="80"
        :rules="groupnameRules"
        label="Environmental Group Name"
        required
      ></v-text-field>
  
      <v-text-field
        v-model="email"
        :rules="emailRules"
        label="E-mail"
        required
      ></v-text-field>
  
      <v-select
        v-model="grouptype"
        :items="items"
        :rules="[v => !!v || 'Item is required']"
        label="Environmental Group Type"
        required
      ></v-select>

      <v-text-field
        v-model="description"
        :counter="150"
        label="Group Description"
        rows="1"
        required
      ></v-text-field>
  
      <v-checkbox
        v-model="checkbox"
        :rules="[v => !!v || 'You must confirm to continue!']"
        label="Confirm group details."
        required
      ></v-checkbox>
  
      <v-btn
        color="success"
        class="mr-4"
        @click="validate"
      >
        Submit
      </v-btn>
  
      <v-btn
        color="error"
        class="mr-4"
        @click="reset"
      >
        Reset Form
      </v-btn>
    </v-form>
  </template>

<script>
import axios from "axios";

export default {
  data: () => ({
    valid: false,
    id: "",
    details:
    {
    groupname: '',
    groupnameRules: [
      v => !!v || 'Name is required',
      v => (v && v.length <= 80) || 'Name must be less than 10 characters',
    ],
    email: '',
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
    ],
    grouptype: null ,
    items: [
      'Wildlife Preservation',
      'Land Preservation',
      'Ocean Conservation',
      'Climate Change',
    ],
    description:'',
    },

    checkbox: false,
  }),

    async created() {
    try {
      const res = await axios.get(`http://localhost:3000/environmentalgroups`);
      this.groups = res.data;
    } catch (error) {
      console.log(error);
    }
  },
  methods: {

     async handleSubmit ()   {   
      const res = await axios.post(`http://localhost:3000/environmentalgroups`, {
        id: this.id,
        details: {
          groupname:this.groupname,
          email:this.email,
          grouptype:this.grouptype,
          description:this.description,
        },
        activities: {
          activity: this.activity
        },
      });
      this.groups = [...this.groups, res.data];
      this.id = "";
      this.groupname = "";
      this.email = "";
      this.grouptype = "";
      this.description = "";
      this.activity = "";
    }  ,


    reset () {
      this.$refs.form.reset()
    },
  },
}
</script>