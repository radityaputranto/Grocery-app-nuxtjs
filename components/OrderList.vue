<template>
  <b-container fluid>
    <h4>Order List</h4>
    <!-- User Interface controls -->
    <b-row class="mb-3">
      <b-col lg="2" md="4" class="my-1">
        <b-form-group
          label="Per page"
          label-for="per-page-select"
          label-cols-sm="6"
          label-cols-md="6"
          label-cols-lg="6"
          label-align-sm="left"
          label-size="sm"
          class="mb-0"
        >
          <b-form-select
            id="per-page-select"
            v-model="perPage"
            :options="pageOptions"
            size="sm"
          ></b-form-select>
        </b-form-group>
      </b-col>

      <b-col lg="10" md="8" class="my-1">
        <b-form-group
          label="Filter"
          label-for="filter-input"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
          class="mb-0 search-table item-right"
        >
          <b-input-group size="sm">
            <b-form-input
              id="filter-input"
              v-model="filter"
              type="search"
              col="4"
              placeholder="Type to Search"
            ></b-form-input>

            <!-- <b-input-group-append>
                        <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
                        </b-input-group-append> -->
          </b-input-group>
        </b-form-group>
      </b-col>
    </b-row>

    <!-- Main table element -->
    <b-table
      :items="products"
      :fields="fields"
      :current-page="currentPage"
      :per-page="perPage"
      :filter="filter"
      :filter-included-fields="filterOn"
      :sort-by.sync="sortBy"
      :sort-desc.sync="sortDesc"
      :sort-direction="sortDirection"
      stacked="md"
      show-empty
      small
      @filtered="onFiltered"
    >
      <template #cell(actions)="row">
        <b-button
          size="sm"
          @click="info(row.item, row.index, $event.target)"
          class="mr-1"
        >
          Info modal
        </b-button>
        <b-button size="sm" @click="row.toggleDetails">
          {{ row.detailsShowing ? "Hide" : "Show" }} Details
        </b-button>
      </template>

      <template #cell(cart-item)="data">
        <ul v-for="(row, key) in data" :key="key">
          <li v-for="(value, key) in row.carts" :key="key">
            {{ value.products.nama }}
          </li>
        </ul>
      </template>
    </b-table>

    <b-row class="justify-content-end">
      <b-col sm="6" md="3" class="my-1">
        <b-pagination
          v-model="currentPage"
          :total-rows="totalRows"
          :per-page="perPage"
          align="fill"
          size="sm"
          class="my-0"
        ></b-pagination>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      products: [],
      fields: [
        {
          key: "nama",
          label: "Name",
          sortable: true,
          sortDirection: "asc",
        },
        {
          key: "email",
          label: "Email",
          sortable: true,
          class: "text-center",
        },
        {
          key: "email",
          label: "Email",
          sortable: true,
          class: "text-center",
        },
        {
          key: "cart-item",
          label: "Carts",
          class: "text-left",
        },
        /* { key: "actions", label: "Actions" }, */
      ],
      totalRows: 1,
      currentPage: 1,
      perPage: 5,
      pageOptions: [5, 10, 15, { value: 100, text: "All" }],
      sortBy: "",
      sortDesc: false,
      sortDirection: "asc",
      filter: null,
      filterOn: [],
      infoModal: {
        id: "info-modal",
        title: "",
        content: "",
      },
    };
  },
  computed: {
    sortOptions() {
      // Create an options list from our fields
      return this.fields
        .filter((f) => f.sortable)
        .map((f) => {
          return { text: f.label, value: f.key };
        });
    },
  },
  mounted() {
    axios
      .get("http://localhost:8001/orders")
      .then((response) => {
        this.setProdct(response.data);
      })
      .catch(function (error) {
        // handle error
        console.log(error);
      });
    this.totalRows = this.products.length;
  },
  methods: {
    info(item, index, button) {
      this.infoModal.title = `Row index: ${index}`;
      this.infoModal.content = JSON.stringify(item, null, 2);
      this.$root.$emit("bv::show::modal", this.infoModal.id, button);
    },
    resetInfoModal() {
      this.infoModal.title = "";
      this.infoModal.content = "";
    },
    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length;
      this.currentPage = 1;
    },
    setProdct(data) {
      this.products = data;
    },
  },
};
</script> 

<style>
ul {
  padding-left: 20px;
}
</style>