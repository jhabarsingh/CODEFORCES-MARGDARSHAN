<template>
  <v-app
    class="v-app"
  >
    
    <v-card
    class="card"
    :elevation="24"
    >
      <v-alert
        dense
        border="left"
        type="primary"
        v-for="(data, index) in alerts" 
        :key="index"
        v-if='data != ""'
      >
        {{ data }}
      </v-alert>
      
      <v-form
        class="form"
        ref="form"
        v-model="valid"
        lazy-validation
      >
        <v-text-field
          v-model="name1"
          :rules="[validateUser, ...nameRules]"
          label="Profile 1"
          required
          :counter=20
        ></v-text-field>
        
        
        <v-text-field
          v-model="name2"
          :rules="[validateUser, ...nameRules]"
          label="Profile 2"
          required
          class="input"
          :counter=20
        ></v-text-field>

        <v-btn
          :disabled="!valid"
          color="warning"
          class="mr-auto"
          @click="submit"
        >
          Submit
        </v-btn>

      </v-form>
  </v-card>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      valid: false,
      name1: '',
      name2: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 20) || 'Name must be less than 20 characters'
      ],
      alerts: [
        "",
        ""
      ]
    }),
    
    computed: {
      getAlerts(str) {
        return this.alerts;
      }
    },

    methods: {
      validate () {
        this.$refs.form.validate()
      },
      validateUser(user) {
        let info = null;
        let url = "https://codeforces.com/api/user.info?handles=" + user;
        try {
          fetch(url).then(data => data.json()).then(data => {
              info = data.status;
              if(info == "FAILED") return `Invalid username`;
          }).catch((err) => {
              return `Invalid username`;
          });
        } catch(ex) {
          return `Invalid username`;
        }
        
        return true;
      }, 
      submit() {
        if(this.$refs.form.validate()) {

        }
      }
     }
  }
</script>

<style scoped>


  .card {
    min-width:450px;
    min-height: 230px;
    margin:auto;
    padding: 20px;
    margin-top: 100px;
    background: transparent;
    opacity: .8;
  }

  input {
    font-weight: bolder;
  }

  button {
    margin-top: 10px;
  }

  .v-app::before {
    content: '';
    position: absolute;
    top: 0px;
    left: 0px;
    width: 100vw;
    height: 100vh;
    background: url(https://thumbs.gfycat.com/NegligibleImmaculateFantail-size_restricted.gif);
    background-repeat: no-repeat;
    background-size: cover;
  }

  .input {
    margin-top: 10px;
  }

</style>