<template>
  <v-card class="overflow-hidden">
    <v-app-bar
      absolute
      color="#6A76AB"
      dark
      shrink-on-scroll
      prominent
      src="https://picsum.photos/1920/1080?random"
      fade-img-on-scroll
      scroll-target="#scrolling-techniques-3"
    >
      <template v-slot:img="{ props }">
        <v-img
          v-bind="props"
          gradient="to top right, rgba(100,115,201,.7), rgba(25,32,72,.7)"
        ></v-img>
      </template>

      <v-btn icon>
        <v-icon large dark @click="$router.push('/')">mdi-home</v-icon>
      </v-btn>

      <v-app-bar-title>CODEFORCES MARGDARSHAN</v-app-bar-title>

      <v-spacer></v-spacer>

      <v-badge
        bordered
        color="primary"
        icon="mdi-eye"
        overlap
        style="margin:10px;"
      >
        <v-btn
          class="white--text"
          color="primary"
          depressed
        >
          Views
        </v-btn>
      </v-badge>


      <template v-slot:extension>
        <v-tabs align-with-title>
          <v-tab>Div 2.A</v-tab>
          <v-tab>Div 2.B</v-tab>
          <v-tab>Div 2.C</v-tab>
          <v-tab>Div 2.D</v-tab>
          <v-tab>Div 2.E</v-tab>
          <v-tab>Div 1.D</v-tab>
          <v-tab>Div 1.E</v-tab>
        </v-tabs>
      </template>
    </v-app-bar>
    <v-sheet
      id="scrolling-techniques-3"
      class="overflow-y-auto"
      max-height="600"
    >
      <v-container style="height: 1000px;"></v-container>
    </v-sheet>
  </v-card>
</template>

<script>
    export default {
      data: () => ({
        yours_name: null,
        others_name: null,
        yours_submissions: new Map(),
        others_submissions: new Map()
      }),

      methods: {
        getUrl(username) {
          let url = `https://codeforces.com/api/user.status?handle=${username}`
          return url;
        },
        getSubmissions() {
          let temp = window.localStorage.getItem("yours_name");
          if(temp == null) {
            this.yours_name = "JhabarBhati";
          }
          else {
            this.yours_name = temp;
          }

          temp = window.localStorage.getItem("others_name");
          if(temp == null) {
            this.others_name = "JhabarBhati";
          }
          else {
            this.others_name = temp;
          }

          this.initializeData(this.yours_name);
          this.initializeData(this.others_name);
        },

        initializeData(username) {
          fetch(this.getUrl(username)).then(data => data.json())
            .then(datas => {
              let data = datas.result.reverse();
              data = data.filter(e => e.verdict == "OK");
              data = data.map(e => e.problem);
              let temp = new Map();

              for(let i in data) {
                let char = data[i].index.toLowerCase()[0];
                if(temp.has(char)) {
                  let p = temp.get(char);
                  p.push(data[i]);
                  temp.set(char, p);
                }
                else {
                  let p = [];
                  p.push(data[i]);
                  temp.set(char, p);
                }
              }
              this.yours_submissions = temp;
              console.log(temp);
            })
        }
      },
      created() {
        this.getSubmissions();
      }
    }
</script>
