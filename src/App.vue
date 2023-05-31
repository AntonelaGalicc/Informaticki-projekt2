<template>
  <v-app id="inspire">
    <v-app-bar app color="gray" dark>
      <v-spacer></v-spacer>
      <v-row class="nav-links">
        <v-btn
          v-for="link in links"
          :key="link.text"
          text
          :to="link.path"
          class="mt-2"
          cols="12"
          md="3"
        >
          <v-icon></v-icon>
          {{ link.text }}
        </v-btn>
      </v-row>
    </v-app-bar>
    <v-main>
      <router-view></router-view> 
    </v-main>
    <v-bottom-navigation
      app
      overlay-opacity=".75"
      temporary
      color="primary"
      dark
    >
      <v-btn @click="toFacebook()" class="social-icon">
        <v-icon class="icon-facebook">mdi-facebook</v-icon>
      </v-btn>
      <v-btn @click="toInstagram()" class="social-icon">
        <v-icon class="icon-instagram">mdi-instagram</v-icon>
      </v-btn>
      <v-btn @click="toTwitter()" class="social-icon">
        <v-icon class="icon-twitter">mdi-twitter</v-icon>
      </v-btn>
    </v-bottom-navigation>
  </v-app>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    group: null,
    links: [
      {
        text: "Knjige",
        path: "/",
      },
      {
        text: "Prijava",
        path: "/prijava",
      },
      {
        text: "Općenito",
        path: "/opcenito",
      },
    ],
    icons: [
      'mdi-facebook',
      'mdi-instagram',
      'mdi-twitter',
    ],
  }),
  methods: {
    toFacebook: function(){
      window.open('https://www.facebook.com/fpmoz.ba')
    },
    toInstagram: function(){
      window.open('https://www.instagram.com/fpmoz.mostar/?hl=hr')
    },
    toTwitter: function(){
      window.open('https://twitter.com/SVE_Mostar')
    },
  }
}
</script>

<style scoped>
.nav-links {
  display: flex;
  align-items: center;
  gap: 10px;
}

.social-icon {
  margin-right: 8px;
}

#inspire {
  background: url("https://images.unsplash.com/photo-1629196613836-0a7e2541990a?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=834&q=80");
   background-size: cover;
}

.app-bar__nav-icon,
.app-bar__title {
  font-family: 'Arial', sans-serif;
  font-weight: bold;
  color: #333;
}
</style>
