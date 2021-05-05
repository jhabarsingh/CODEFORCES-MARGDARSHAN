<template>
  <v-app
    class="v-app"
  >
    
    <v-card
    class="card"
    :elevation="24"
    >
      
      <v-form
        class="form"
        ref="form"
        v-model="valid"
      >
        <v-alert
          dense
          :elevation="24"
          type="error"
          v-show="display"
        >
          Invalid Username
        </v-alert>

        <v-text-field
          v-model="name1"
          :rules="[...nameRules]"
          label="Your Profile"
          required
          :counter=20
        ></v-text-field>
        
        
        <v-text-field
          v-model="name2"
          :rules="[...nameRules]"
          label="Other's Profile"
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
      display: false,
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 20) || 'Name must be less than 20 characters'
      ],
      
    }),
    
    methods: {
      validateUser(user) {
        let info = null;
        let url = "https://codeforces.com/api/user.info?handles=" + user;
        fetch(url).then(data => data.json()).then(data => {
            let info = data.status;
            console.log(info)
            if(info == "OK") {
              window.localStorage.setItem("yours_name", this.name1);
              window.localStorage.setItem("others_name", this.name2);
              this.$router.push("/submissions");
            }
        })
        return false;
      }, 
      submit() {
        this.validateUser(this.name1 + ";" + this.name2)
        
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
    background: url('../../public/home.gif');
    background-repeat: no-repeat;
    background-size: cover;
  }

  .input {
    margin-top: 10px;
  }

</style>