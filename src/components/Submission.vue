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
          <v-tab @click="index='a'">Div 2.A</v-tab>
          <v-tab @click="index='b'">Div 2.B</v-tab>
          <v-tab @click="index='c'">Div 2.C</v-tab>
          <v-tab @click="index='d'">Div 2.D</v-tab>
          <v-tab @click="index='e'">Div 2.E</v-tab>
          <v-tab @click="index='f'">Div 1.D</v-tab>
          <v-tab @click="index='g'">Div 1.E</v-tab>
        </v-tabs>
      </template>
    </v-app-bar>
    <v-sheet
      id="scrolling-techniques-3"
    >
      <v-container style="min-height: 1000px;margin-top:250px">
        <List :index='index'/>
      </v-container>
    </v-sheet>
  </v-card>
</template>

<script>
    import List from './List'
    export default {
      components: {
        List,
      },
      
      data: () => ({
        index: 'a'
      }),

      methods: {
        
      }
    }
    // https://codeforces.com/api/contest.ratingChanges?contestId=566
</script>
