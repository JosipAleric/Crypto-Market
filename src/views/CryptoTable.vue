<template>
  <div class="table">
    <!-- Info section start -->
    <!-- Main heading -->
    <h3 class="text-h4 for xs text-sm-h4 text-md-h3 text-lg-h3 text-center">
      Cryptocurrency Market 
    </h3>
    <br />

    <!-- Paragraf -->
    <p class="text-center body-1 pt-2 pb-2">
      Get all the information about crypto prices, market cap, total volume, and
      market related data in real time.
    </p>
    <br />

    <!-- Crypto info -->
    <v-row style="padding: 0% 10%">
      <v-col
        cols="12"
        lg="4"
        md="4"
        sm="12"
        class="text-center text-body-1 "
      >
        <h3>
          Cryptocurrencies: <br /><span style="color: #e91e63"
            >{{ APIdata.length }} Coins</span
          >
        </h3>
      </v-col>
      <v-col
        class="text-center text-body-1 "
        cols="12"
        lg="4"
        md="4"
        sm="12"
      >
        <h3>
          BTC Current Price: <br /><span style="color: #e91e63"
            >{{
              APIdata[0].current_price.toLocaleString("en-US", {
                style: "currency",
                currency: "USD",
              })
            }}
            USD
          </span>
        </h3>
      </v-col>
      <v-col
        class="text-center text-body-1"
        cols="12"
        lg="4"
        md="4"
        sm="12"
      >
        <h3>
          BTC Total Volume: <br /><span style="color: #e91e63"
            >{{ formatPrice(APIdata[0].total_volume) }} USD
          </span>
        </h3>
      </v-col>
    </v-row>
    <br />
    <!-- Info section end -->

    <v-divider></v-divider> <br />
    <v-card :loading="loading">
      <!-- Table heading -->
      <v-card-title>
        <div class="v-card__title align-start mt-n3">
          <div
            class="
              overflow-hidden
              mt-n7
              mb-n4
              transition-swing
              v-card--material__sheet
              v-sheet
              primary
              elevation-7
              rounded
            "
            style="max-width: 100%;"
          >
            <div class="pa-7">
              <i
                aria-hidden="true"
                class="v-icon notranslate mdi mdi-clipboard-text theme--dark"
              >
              </i>
            </div>
          </div>
         <h3 class="font-weight-light pl-3" >
          Cryptocurrencies
        </h3>
        </div>
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
        :items="APIdata"
        :page.sync="page"
        @page-count="pageCount = $event"
        :search="search"
        :hide-default-footer="true"
        items-per-page="12"
        class="elevation-1"
        :loading="loading"
      >
        <!-- Table ID column -->
        <template v-slot:[`item.count`]="{ item }">
          <td>
            {{
              APIdata.map(function (x) {
                return x.id;
              }).indexOf(item.id) + 1
            }}
          </td>
        </template>

        <!-- Table Name column -->
        <template v-slot:[`item.name`]="{ item }">
          <td class="text-lg-left font-weight-medium body-1">
            <img
              :src="item.image"
              width="38px"
              height="38px"
              class="mr-4 mt-2 mb-2"
              style="vertical-align: middle"
            />
            <span class="mr-4" style="font-size: 15px"> {{ item.name }} </span>
          </td>
        </template>

        <!-- Table Current Price column -->
        <template v-slot:[`item.current_price`]="{ item }">
          <td class="body-2">
            {{
              item.current_price.toLocaleString("en-US", {
                style: "currency",
                currency: "USD",
              })
            }}
          </td>
        </template>

        <!-- Table Market Cap column -->
        <template v-slot:[`item.market_cap`]="{ item }">
          <td class="body-2">
            {{ formatPrice(item.market_cap) }}
          </td>
        </template>

        <!-- Table Total Volume column -->
        <template v-slot:[`item.total_volume`]="{ item }">
          <td class="body-2">
            {{ formatPrice(item.total_volume) }}
          </td>
        </template>

        <!-- Table Symbol column -->
        <template v-slot:[`item.symbol`]="{ item }">
          <td class="body-2 font-weight-bold">
            <span style="color: grey">{{ item.symbol.toUpperCase() }} </span>
          </td>
        </template>

        <!-- Table 24h Price Change column -->
        <template v-slot:[`item.price_change_percentage_24h`]="{ item }">
          <td class="body-2">
            <v-chip :color="getColor(item.price_change_percentage_24h)" dark>
              {{ item.price_change_percentage_24h.toFixed(2) }} %
            </v-chip>
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
    APIdata: [],
    search: "",
    page: 1,
    pageCount: 0,
    loading: true,
    headers: [
      {
        text: "ID",
        align: "start",
        sortable: false,
        value: "count",
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
        text: "Current Price",
        value: "current_price",
        class: "subtitle-1 pink--text title",
      },
      {
        text: "Market Cap",
        value: "market_cap",
        class: "subtitle-1 pink--text title",
      },
      {
        text: "24h Price change",
        value: "price_change_percentage_24h",
        class: "subtitle-1 pink--text title",
      },
      {
        text: "Total Volume",
        value: "total_volume",
        class: "subtitle-1 pink--text title",
      },
      {
        text: "Symbol",
        value: "symbol",
        class: "subtitle-1 pink--text title",
      },
    ],
  }),

  methods: {
    //Fetching data from API
    getData: function () {
      this.axios
        .get(
          "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
        )
        .then((response) => {
          console.log(response.data);
          this.APIdata = response.data;
          this.loading = false;
        });
    },

    //getColor for Chip method
    getColor(number) {
      if (number < 0) return "#e82857";
      else return "green";
    },

    //Format prices from API
    formatPrice: function (price) {
      return price
        .toLocaleString("en-US", {
          style: "currency",
          currency: "USD",
        })
        .slice(0, -3);
    },
  },

  mounted() {
    this.getData();
    document.title = "Cryptocurrencies";
  },
};
</script>

<style>
::v-deep .v-data-table-header {
  font-size: 15px;
}
</style>
