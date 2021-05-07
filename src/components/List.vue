<template>
  <v-card
    class="mx-auto"
    max-width="600"
    :elevation="3"
    v-if="items.length != 0"
  >
    <v-list class="list">
      <v-list-item-group v-model="model">
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :class="finder(item.contestId) ? 'light-blue lighten-2' : ''"
          @click="solveQuestion(item.contestId, item.index)"
          style="margin-top:1px;"
        >
          <v-list-item-icon>        
            <v-icon
            md
            color="green darken-2"
            v-if="finder(item.contestId)"
            >
                mdi-checkbox-marked-circle
            </v-icon>
            <v-icon
            md
            color="green darken-2"
            v-else
            >
                mdi-pencil
            </v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title v-text="item.name"></v-list-item-title>
          </v-list-item-content>
          <v-btn
            class="mx-1"
            color="error"
            @click="messages++"
            v-if="finder(item.contestId)"
          >
            {{item.rating == null ? "NR" : item.rating}}
          </v-btn>
          <v-btn
            class="mx-1"
            color="warning"
            @click="messages++"
            v-else
          >
            {{item.rating == null ? "NR" : item.rating}}
          </v-btn>
        </v-list-item>
      </v-list-item-group>
    </v-list>
  </v-card>
  <v-alert
    border="left"
    color="warning"
    dense
    type="success"
    v-else
  >No Question</v-alert>
</template>


<script>
  export default {
    data: () => ({
      items: [
      ],
      items1: [

      ],
      model: 1,
      yours_submissions: null,
      others_submissions: null,
      yours_name: 'JhabarBhati',
      others_name: 'JhabarBhati',
      repeated: new Set()
    }),
    
    props: [
        'index'
    ],

    methods: {
        getContestDetails(id, index) {
          return url;
        },
        solveQuestion(id, index) {
          let url = `https://codeforces.com/problemset/problem/${id}/${index}`
          window.open(url, "_black");  
        },
        finder(id) {
          return this.repeated.has(id);
        },
        getUrl(username) {
          let url = `https://codeforces.com/api/user.status?handle=${username}`
          return url;
        },
        getRepeated() {
          if(this.items.length == 0 || this.items1.length == 0) {
            return;
          }
          else {
            let temp = new Set();
            for(let i=0; i<this.items.length; i++) {
              for(let j=0; j<this.items1.length; j++) {
                if(this.items[i].contestId == this.items1[j].contestId) {
                  temp.add(this.items[i].contestId);
                }
              } 
            }
            this.repeated = temp;
          }
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
          this.initializeData(this.yours_name, true);
          this.initializeData(this.others_name, false);
        },

        initializeData(username, isyou) {
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
              if(isyou) {
                this.yours_submissions = temp
                if(temp != null) {
                  temp = temp.get('a').sort((a, b) => +a.rating - +b.rating);
                  this.items1 = temp.sort((a, b) => +a.rating - +b.rating);
                }
              }
              else {
                this.others_submissions = temp;
                if(temp != null) {
                  temp = temp.get('a');
                  this.items = temp.sort((a, b) => +a.rating - +b.rating);
                  this.getRepeated();
                }
              }
            })
        }
    },

    watch: {
      index(val) {
        let temp = this.others_submissions.get(val);
        if(temp == null) {
          this.items = [];
        }
        else {
          this.items = temp.sort((a, b) => +a.rating - +b.rating);
        }
        
        temp = this.yours_submissions.get(val);

        if(temp == null) {
          this.items1 = [];
        }
        else {
          this.items1 = temp.sort((a, b) => +a.rating - +b.rating);
          this.getRepeated();
        }
      }
    },

    mounted() {
      console.log(this.index);
    },
    
    created() {
      this.getSubmissions();
    }
  }
</script>

<style scoped>
  .list {
    padding: 10px;
  }
</style>