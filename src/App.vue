<template>
  <v-app id="inspire">
    <!-- Logo and title start -->
    <v-navigation-drawer v-model="drawer" app color="#1E1E1E">
      <v-sheet>
        <v-list
          style="
            margin-top: -10px;
            margin-bottom: -10px;
            background-color: #1e1e1e;
          "
        >
          <v-list-item>
            <v-list-item-icon>
              <img
                src="../logo.png"
                height="40px"
                width="40px"
                style="margin-right: -15px"
              />
            </v-list-item-icon>

            <v-list-item-content class="font-weight-thin">
              <strong class="mr-1 font-weight-black pink--text title heading-1"
                >CRYPTO MARKET</strong
              >
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-sheet>
      <!-- Logo and title end -->

      <v-divider></v-divider>

      <!-- Navigation start -->
      <v-list rounded flat>
        <v-list-item
          v-for="[icon, text, route] in links"
          :key="icon"
          :to="route"
          link
        >
          <v-list-item-icon>
            <v-icon>{{ icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title id="li">{{ text }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <!-- Footer -->
      <v-footer max-width="99%" absolute color="#1E1E1E" class="mb-15">
        <v-col class="text-center white--text" cols="12">
          Made by <br />
          <strong>Josip Alerić</strong>
        </v-col>
      </v-footer>
    </v-navigation-drawer>
    <!-- Navigation end -->

    <!-- Main section start -->
    <v-main>
      <!-- App bar section start -->
      <v-row class="d-lg-none">
        <v-col>
          <div>
            <v-app-bar
              dense
              elevation="15"
              color="#121212"
              fixed
            >
              <v-btn @click="drawer = !drawer" icon>
                <v-app-bar-nav-icon></v-app-bar-nav-icon>
              </v-btn>
              <v-toolbar-title>{{naslov}}</v-toolbar-title>
              <v-spacer></v-spacer>

              <v-btn icon href="/">
                <v-icon>mdi-home</v-icon>
              </v-btn>

              <v-btn icon>
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </v-app-bar>
          </div>
        </v-col>
      </v-row>
      <!-- App bar section end -->

      <v-container :class="{ 'py-15 px-15':$vuetify.breakpoint.mdAndUp, 'py-15 px-5': $vuetify.breakpoint.xsOnly}">
        <router-view />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    cards: ["Today", "Yesterday"],
    drawer: true,
    naslov: "",
    links: [
      ["mdi-home", "Home", "/"],
      ["mdi-bitcoin", "Cryptocurrencies", "/table"],
      ["mdi-currency-usd", "Crypto Exchanges", "/exchanges"],
    ],
  }),

  methods: {
  },

  updated(){
    this.naslov = document.title
  }
};
</script>

<style>
.v-list-item--active {
  background-color: #e91e63;
  color: white;
}
</style>


