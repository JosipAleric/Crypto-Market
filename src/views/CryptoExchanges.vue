<template>
  <div class="exchanges">
    <!-- Info section start -->
    <!-- Main heading -->
    <h3 class="text-h4 for xs text-sm-h4 text-md-h3 text-lg-h3 text-center">
      Cryptocurrency Exchanges
    </h3>
    <br />

    <!-- Paragraf -->
    <p class="text-center body-1 pt-2 pb-2" >
      Crypto exchanges provide you with the tools you need to buy and sell digital currencies and tokens like Bitcoin, Ethereum, and Dogecoin. <br class="d-lg-none"> <br>
      Buy, sell and trade crypto coins and tokens with these top exchanges for virtual currency transactions.
    </p>
    <br />

    <v-divider></v-divider> <br />
    <v-card>
      <!-- Table heading -->
      <v-card-title>
        <h3 class="font-weight-medium pl-3" style="color: #e91e63">
          Exchanges
        </h3>
        <v-spacer></v-spacer>

        <!-- Table Search bar -->
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>

      <!-- Data Table start -->
      <v-data-table
        :headers="headers"
        :items="APIExchangesData"
        :page.sync="page"
        @page-count="pageCount = $event"
        :search="search"
        :hide-default-footer="true"
        items-per-page="12"
        class="elevation-1"
        :loading="loading"
      >
        <!-- Table Rank column -->
        <template v-slot:[`item.rank`]="{ item }">
          <td>
            {{ item.trust_score_rank }}
          </td>
        </template>

        <!-- Table Name column -->
        <template v-slot:[`item.name`]="{ item }">
          <td class="text-lg-left font-weight-medium body-1">
            <a :href="item.url" target="blank">
            <img
              :src="item.image"
              width="38px"
              height="38px"
              class="mr-4 mt-2 mb-2"
              style="vertical-align: middle"
            />
            </a>
            <span class="mr-4" style="font-size: 15px"> {{ item.name }} </span>
          </td>
        </template>

        <!-- Table Trust Score column -->
        <template v-slot:[`item.trust_score`]="{ item }">
          <v-chip :color="getColor(item.trust_score)" dark>
            {{ item.trust_score }}
          </v-chip>
        </template>

        <!-- Table Trade Volume column -->
        <template v-slot:[`item.trade_volume_24h_btc`]="{ item }">
          <td class="body-2">
            {{
              item.trade_volume_24h_btc.toLocaleString("en-US", {
                valute: "USD",
              }).slice(0, -4)
            }} <span>BTC</span> 
          </td>
        </template>

        <!-- Table Country column -->
        <template v-slot:[`item.country`]="{ item }">
          <td class="body-2">
            {{ item.country }}
          </td>
        </template>

        <!-- Table Year Established column -->
        <template v-slot:[`item.year_established`]="{ item }">
          <td class="body-2 font-weight-bold">
            <span style="color: grey">{{ item.year_established }} </span>
          </td>
        </template>
      </v-data-table></v-card
    >
    <!-- Data Table end -->

    <!-- Pagination -->
    <div class="text-center ma-5">
      <v-pagination
        v-model="page"
        :length="pageCount"
        :total-visible="7"
        circle
      ></v-pagination>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    APIExchangesData: [],
    search: "",
    page: 1,
    pageCount: 0,
    loading: true,
    headers: [
      {
        text: "Rank",
        align: "start",
        sortable: false,
        value: "trust_score_rank",
        class: "subtitle-1 pink--text title",
      },
      {
        text: "Name",
        align: "start",
        sortable: false,
        value: "name",
        class: "subtitle-1 pink--text title",
      },
      {
        text: "Trust Score",
        value: "trust_score",
        class: "subtitle-1 pink--text title",
      },
      {
        text: "24h Trade Volume",
        value: "trade_volume_24h_btc",
        class: "subtitle-1 pink--text title",
      },
      {
        text: "Country",
        value: "country",
        class: "subtitle-1 pink--text title",
      },
      {
        text: "Year Established",
        value: "year_established",
        class: "subtitle-1 pink--text title",
      },
    ],
  }),

  methods: {
    //Fetching data from API
    getExchangesData: function () {
      this.axios
        .get("https://api.coingecko.com/api/v3/exchanges?per_page=100&page=1")
        .then((response) => {
          console.log(response.data);
          this.APIExchangesData = response.data;
          this.loading = false;
        });
    },

    //getColor for Chip method
    getColor(number) {
      if (number <= 4) {
        return "#e82857";
      } else if (number <= 7) {
        return "orange";
      } else {
        return "green";
      }
    },

    //Format prices from API
    formatPrice: function (price) {
      return price.toLocaleString("en-US", {
        style: "currency",
      });
    },
  },

  mounted() {
    this.getExchangesData();
    document.title = "Crypto Exchanges";
  },
};
</script>

<style>
::v-deep .v-data-table-header {
  font-size: 15px;
}
</style>