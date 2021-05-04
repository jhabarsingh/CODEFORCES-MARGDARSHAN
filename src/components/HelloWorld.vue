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
        lazy-validation
        @submit.prevent
      >
        <v-text-field
          v-model="name1"
          :rules="[validateUser, ...nameRules]"
          label="Profile 1"
          required
          :counter=20
          @keyup.enter="submit"
        ></v-text-field>

        <v-text-field
          v-model="name2"
          :rules="[v => validateUser(v), ...nameRules]"
          label="Profile 2"
          required
          :counter=20
          @keyup.enter="submit"
        ></v-text-field>


        <v-btn
          :disabled="!valid"
          color="success"
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
      ]
    }),

    methods: {
      validateUser(user) {
        let info = null;
        let url = "https://codeforces.com/api/user.info?handles=" + user;
        fetch(url).then(data => data.json()).then(data => {
            info = data.status;
        });
        console.log(info);
        if(info == "FAILED") return `${user} does not exist`;
        return true;
      }, 
      submit() {
        this.$refs.form.validate()
      }
     }
  }
</script>

<style scoped>


  .card {
    min-width:500px;
    min-height: 230px;
    margin:auto;
    padding: 20px;
    margin-top: 100px;
    background: transparent;
    opacity: .6;
  }

  input {
    font-weight: bolder;
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