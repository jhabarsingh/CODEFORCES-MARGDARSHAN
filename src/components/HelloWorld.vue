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
        v-for="(data, index) in alerts" :key="index"
      >
        {{ data }}
      </v-alert>
      
      <v-form
        class="form"
        ref="form"
        v-model="valid"
        lazy-validation
        @submit.prevent
      >
        <v-text-field
          v-model="name1"
          :rules="[...nameRules]"
          label="Profile 1"
          required
          :counter=20
          @keyup.enter="submit"
        ></v-text-field>

        <v-text-field
          v-model="name2"
          :rules="[...nameRules]"
          label="Profile 2"
          required
          :counter=20
          @keyup.enter="submit"
        ></v-text-field>


        <v-btn
          :disabled="!valid"
          color="warning"
          class="mr-4"
          @click="submit"
        >
          Validate
        </v-btn>

      </v-form>
  </v-card>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      valid: true,
      name1: '',
      name2: '',
      nameRules: [
        v => !!v || 'Name is required'
      ],
      alerts: [

      ]
    }),

    methods: {
      validateUser(user) {
        let info = null;
        let url = "https://codeforces.com/api/user.info?handles=" + user;
        let option = {
          mode: "no-cors"
        }
        fetch(url, option).then(data => data.json()).then(data => {
            info = data.status;
        });
        console.log(info);
        if(info == "FAILED") return false;
        return true;
      }, 
      submit() {
        if(!this.validateUser(this.name1)) {
          this.alerts.push(`${name1} does not exist`);
        }
        if(!this.validateUser(this.name2)) {
          this.alerts.push(`${name2} does not exist`);
        }
        else {
          return true;
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

</style>