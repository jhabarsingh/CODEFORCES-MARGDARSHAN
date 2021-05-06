<template>
  <v-footer
    dark
    padless
    style="margin-top:50px;"
  >
    <v-card
      flat
      tile
      class="indigo lighten-1 white--text text-center"
      style="width:100%;"
    >
      <v-card-text>
        <v-btn
          v-for="(icon, index) in icons"
          :key="icon"
          class="mx-4 white--text"
          icon
          @click="goTo(index)"
        >
          <v-icon size="24px">
            {{ icon }}
          </v-icon>
        </v-btn>
      </v-card-text>

      <v-card-text class="white--text pt-0" style="font-size:15px;">
        {{footer}}
      </v-card-text>

      <v-divider></v-divider>

      <v-card-text class="white--text">
        {{ new Date().getFullYear() }} — <strong>CODEFORCES MARGDARSHAN</strong>
      </v-card-text>
    </v-card>
  </v-footer>
</template>

<script>
  export default {
    data: () => ({
      icons: [
        'mdi-facebook',
        'mdi-twitter',
        'mdi-linkedin',
        'mdi-instagram',
        'mdi-github'
      ],
      links: [
          'https://www.facebook.com/jabbe.don/',
          'https://twitter.com/BhatiJhabar',
          'https://www.linkedin.com/in/jhabar-bhati-774969134/',
          'https://www.instagram.com/bhatijhabarsingh/',
          'https://github.com/jhabarsingh/CODEFORCES-MARGDARSHAN'
      ],
      footer: "Success is nothing more than a few simple disciplines, practiced every day."
    }),

    methods: {
        goTo(index) {
            window.location.href = this.links[index];
        }
    }
  }
</script>