<template>
  <div>
    <h3 class="text-h4 for xs text-sm-h4 text-md-h3 text-lg-h3 text-center" style="padding-bottom: 30px;">
      Latest Crypto News
    </h3>
    <v-divider style="padding-bottom: 40px;" ></v-divider>

    <!-- Carousel -->
    <v-carousel cycle interval="4000" progress progress-color="#e91e63" height="550px" hide-delimiters>
      <!-- Main news -->
      <v-carousel-item v-for="i in 3" :key="i" transition="fade-transition" reverse-transition="fade-transition">
        <div> 
          <v-card target="blank" :href="APINewsData[i].url">
            <v-img
              :aspect-ratio="16 / 9"
              dark
              gradient="to top, rgba(25,32,72,.7), rgba(25,32,72,.0)"
              height="550px"
              :src="APINewsData[i].urlToImage"
            >
              <v-card-text class="fill-height d-flex align-end" >
                <v-row class="flex-column">
                  <v-col cols="12" lg="8" md="10" xl="7">
                    <h2 class="py-3 text-h4 for xs text-sm-h4 text-md-h4 text-lg-h3" style="line-height: 1.2" >
                      {{ APINewsData[i].title }}
                    </h2>
                  </v-col>
                  <v-col class="d-flex align-center" >
                    <v-avatar class="elevation-4" color="#e91e63" :class="{'size=10': $vuetify.breakpoint.mobile}">
                      <v-icon large>mdi-feather</v-icon>
                    </v-avatar>

                    <div class="text-h6 pl-2" :class="{'text-body-1 ': $vuetify.breakpoint.mobile}">
                      {{ APINewsData[i].author }} ·
                      {{ APINewsData[i].publishedAt.slice(0, -10) }}
                    </div>
                  </v-col>
                </v-row>
              </v-card-text>
            </v-img>
          </v-card>
        </div>
      </v-carousel-item>
    </v-carousel>
    <!-- Carousel end -->

    <!-- Latest Posts news -->
    <v-row>
      <v-col cols="12" lg="12" xl="8">
        <div>
          <div class="pt-16">
            <h2 class="text-h4 font-weight-bold pb-4">Latest Posts</h2>
            <v-divider></v-divider>

            <div>
              <v-row v-for="i in 3" :key="i" class="py-4">
                <v-col cols="12" md="4">
                  <v-card flat height="100%" >
                    <v-img
                      :aspect-ratio="16 / 9"
                      height="100%"
                      :src="APINewsData[i + 3].urlToImage"
                    ></v-img>
                  </v-card>
                </v-col>

                <v-col>
                  <div>
                    <v-btn
                      color="#e91e63"
                      target="blank"
                      :href="APINewsData[i + 3].url"
                      link
                      depressed
                      >Learn More</v-btn
                    >

                    <h3 class="text-h4 font-weight-bold pt-3">
                      {{ APINewsData[i + 3].title }}
                    </h3>

                    <p class="text-h6 font-weight-regular pt-3 text--secondary">
                      {{ APINewsData[i + 3].content.slice(0, -13) }}
                    </p>

                    <div class="d-flex align-center">
                      <v-avatar color="#e91e63" size="36">
                        <v-icon dark>mdi-feather</v-icon>
                      </v-avatar>

                      <div class="pl-2">
                        {{ APINewsData[i + 3].author }} ·
                        {{ APINewsData[i + 3].publishedAt.slice(0, -10) }}
                      </div>
                    </div>
                  </div>
                </v-col>
              </v-row>
            </div>
          </div>
        </div>
      </v-col>
      <!-- Latest Posts news end -->

      <!-- Popular Posts news -->
      <v-col>
        <div class="pt-16">
          <h3 class="text-h4 font-weight-bold pb-4">Popular Posts</h3>

          <v-divider></v-divider>

          <div>
            <v-row v-for="i in 5" :key="i" class="py-4">
              <v-col cols="12" md="6" lg="5">
                <v-card height="100%" flat>
                  <v-img
                    :src="APINewsData[i + 5].urlToImage"
                    :aspect-ratio="16 / 9"
                    height="100%"
                  ></v-img>
                </v-card>
              </v-col>

              <v-col>
                <div>
                  <v-btn
                    color="#e91e63"
                    target="blank"
                    :href="APINewsData[i + 5].url"
                    link
                    depressed
                    small
                    >Learn More</v-btn
                  >

                  <h3 class="text-h6 font-weight-bold py-3">
                    {{ APINewsData[i + 5].title }}
                  </h3>

                  <div class="d-flex align-center">
                    <v-avatar color="accent" size="24">
                      <v-icon dark small>mdi-feather</v-icon>
                    </v-avatar>

                    <div class="pl-2">
                      {{ APINewsData[i + 5].author }} ·
                      {{ APINewsData[i + 5].publishedAt.slice(0, -10) }}
                    </div>
                  </div>
                </div>
              </v-col>
            </v-row>
          </div>
        </div>
      </v-col>
      <!-- Popular Posts news end -->
    </v-row>
  </div>
</template>

<script>
export default {
  name: "NewsView",
  components: {},
  data: () => ({
    APINewsData: [],
  }),

  methods: {
    //Fetching data from API
    getNewsData: function () {
      this.axios
        .get(
          "https://newsapi.org/v2/everything?q=bitcoin&apiKey=b98918b4ae4641c88a7056dbfbd8b505"
        )
        .then((response) => {
          console.log(response.data);
          this.APINewsData = response.data.articles;
        });
    },

    //Printing data from API
    ispis: function () {
      console.log(this.APINewsData);
    },
  },

  mounted() {
    this.getNewsData();
    document.title = "Home";
  },
};
</script>

