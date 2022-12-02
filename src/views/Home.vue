<template>
  <v-container fluid>
    <v-row>
      <v-col cols="2">
        <v-card outlined>
          <v-card class="mx-auto" max-width="400">
            <v-card-title> Filters</v-card-title>
            <v-divider />

            <!-- :prepend-icon="item.action" -->
            <v-list>
              <v-list-group
                v-for="item in filters"
                :key="item.filter_lable"
                v-model="item.active"
                no-action
              >
                <template v-slot:activator>
                  <v-list-item-content>
                    <v-list-item-title v-text="item.filter_lable" />
                  </v-list-item-content>
                </template>
                <v-list-item-group multiple>
                  <v-list-item
                    v-for="child in item.options"
                    :key="child.frequent_filter_count"
                    class="pl-4"
                    @click="onClick(child, item.active)"
                  >
                    <template v-slot:default="{ active }">
                      <v-list-item-action class="mr-2">
                        <v-checkbox :input-value="active" />
                      </v-list-item-action>
                      <v-list-item-content>
                        <v-list-item-title v-text="child.value" />
                      </v-list-item-content>
                    </template>
                  </v-list-item>
                </v-list-item-group>
              </v-list-group>
            </v-list>
          </v-card>
        </v-card>
      </v-col>
      <!-- <v-skeleton-loader v-if="loading" /> -->
      <v-col cols="10">
        <v-card outlined>
          <v-row>
            <v-col cols="12">
              <p class="pa-4 title">{{ totalCount }} items</p>
            </v-col>
            <v-divider />
            <v-skeleton-loader v-if="loading" cols="12"></v-skeleton-loader>
            <v-col
              xs="12"
              md="3"
              sm="4"
              cols="12"
              v-for="item in products"
              :key="item.id_product"
              v-else
            >
              <ProductCard :item="item" />
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import ProductCard from "../components/ProductCard.vue";
import axios from "axios";

export default {
  name: "Home",
  data: () => ({
    totalCount: 0,
    pageNo: 1,
    API_URL1:
      "https://pim.wforwoman.com/pim/pimresponse.php/?service=category&store=1&url_key=top-wear-kurtas&page=",
    products: [],
    API_URL2: "&count=20&sort_by=&sort_dir=desc&filter=",
    filters: [],
    testItem: [],
    loading: true,
    filter: "",
  }),
  components: { ProductCard },
  mounted() {
    this.fetchData(this.API_URL1 + 1 + this.API_URL2);
    this.fetchFilters();
    this.getNextUser();
  },
  methods: {
    getNextUser() {
      window.onscroll = () => {
        let bottomOfWindow =
          document.documentElement.scrollTop + window.innerHeight >=
          document.documentElement.offsetHeight - 1;
        if (bottomOfWindow) {
          console.log("At bottom");
          this.pageNo++;
          this.fetchData(this.API_URL1 + this.pageNo + this.API_URL2);
        }
      };
    },
    fetchData(url) {
      this.loading = true;
      if (this.pageNo == 1) {
        this.products = [];
      }
      axios.get(url).then((response) => {
        console.log("response.data :>> ", response.data);
        this.products = this.products.concat(response.data.result.products);
        this.totalCount = response.data.result.count;
        // this.filters = response.data.result.filters;
        this.loading = false;
      });
    },
    fetchFilters() {
      axios.get(this.API_URL1 + 1 + this.API_URL2).then((response) => {
        this.filters = response.data.result.filters;
      });
    },
    onClick(item) {
      if (this.filter.includes(item.code + "-" + item.value)) {
        this.filter = this.filter.replace(
          item.code + "-" + item.value + ",",
          ""
        );
        this.filter = this.filter.replace(
          "," + item.code + "-" + item.value,
          ""
        );
        this.filter = this.filter.replace(item.code + "-" + item.value, "");
      } else {
        if (this.filter)
          this.filter = this.filter + "," + item.code + "-" + item.value;
        else this.filter = item.code + "-" + item.value;
      }
      console.log("filter :>> ", this.filter);
      let url = this.API_URL1 + this.pageNo + this.API_URL2 + this.filter;
      this.fetchData(url);
    },
  },
};
</script>
