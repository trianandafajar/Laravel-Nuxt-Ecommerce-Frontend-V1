<template>
  <div class="container-fluid mt-custom">
    <div class="fade-in">
      <div class="row">
        <!-- Sidebar -->
        <div class="col-md-3">
          <Sidebar />
        </div>

        <!-- Main Content -->
        <div class="col-md-9">
          <div class="card border-0 rounded shadow-sm border-top-orange">
            <div class="card-body">
              <h5><i class="fa fa-shopping-cart"></i> MY ORDERS</h5>
              <hr />

              <!-- Search -->
              <div class="form-group">
                <div class="input-group mb-3">
                  <input
                    type="text"
                    class="form-control"
                    v-model="search"
                    @keypress.enter="searchData"
                    placeholder="Cari berdasarkan No. Invoice"
                  />
                  <div class="input-group-append">
                    <button @click="searchData" class="btn btn-warning">
                      <i class="fa fa-search"></i> SEARCH
                    </button>
                  </div>
                </div>
              </div>

              <!-- Table -->
              <b-table
                striped
                bordered
                hover
                :items="invoices.data"
                :fields="fields"
                show-empty
                empty-text="Belum ada invoice ditemukan"
              >
                <!-- Format Grand Total -->
                <template v-slot:cell(grand_total)="row">
                  Rp. {{ formatPrice(row.item.grand_total) }}
                </template>

                <!-- Format Status -->
                <template v-slot:cell(status)="row">
                  <span v-if="row.item.status === 'pending'" class="btn btn-sm btn-primary">
                    <i class="fa fa-circle-notch fa-spin"></i> {{ row.item.status }}
                  </span>
                  <span v-else-if="row.item.status === 'success'" class="btn btn-sm btn-success">
                    <i class="fa fa-check-circle"></i> {{ row.item.status }}
                  </span>
                  <span v-else-if="row.item.status === 'expired'" class="btn btn-sm btn-warning-2">
                    <i class="fa fa-exclamation-triangle"></i> {{ row.item.status }}
                  </span>
                  <span v-else class="btn btn-sm btn-danger">
                    <i class="fa fa-times-circle"></i> {{ row.item.status }}
                  </span>
                </template>

                <!-- Actions -->
                <template v-slot:cell(actions)="row">
                  <b-button
                    :to="{
                      name: 'customer-invoices-show-snap_token',
                      params: { snap_token: row.item.snap_token },
                    }"
                    variant="info"
                    size="sm"
                  >
                    DETAIL
                  </b-button>
                </template>
              </b-table>

              <!-- Pagination -->
              <b-pagination
                align="right"
                :value="invoices.current_page"
                :total-rows="invoices.total"
                :per-page="invoices.per_page"
                @change="changePage"
                aria-controls="my-table"
              ></b-pagination>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Sidebar from "@/components/web/sidebar.vue";

export default {
  middleware: "isCustomer",
  layout: "default",

  components: {
    Sidebar,
  },

  head() {
    return {
      title: "Invoices - Customer",
    };
  },

  data() {
    return {
      fields: [
        { label: "No. Invoice", key: "invoice" },
        { label: "Grand Total", key: "grand_total" },
        { label: "Status Payment", key: "status", tdClass: "text-center" },
        { label: "Actions", key: "actions", tdClass: "text-center" },
      ],
      search: "",
    };
  },

  async asyncData({ store }) {
    await store.dispatch("customer/invoice/getInvoicesData");
  },

  computed: {
    invoices() {
      return this.$store.state.customer.invoice.invoices || {
        data: [],
        total: 0,
        current_page: 1,
        per_page: 10,
      };
    },
  },

  methods: {
    searchData() {
      this.$store.commit("customer/invoice/SET_PAGE", 1);
      this.$store.dispatch("customer/invoice/getInvoicesData", this.search);
    },

    changePage(page) {
      this.$store.commit("customer/invoice/SET_PAGE", page);
      this.$store.dispatch("customer/invoice/getInvoicesData", this.search);
    },

    formatPrice(value) {
      return new Intl.NumberFormat("id-ID").format(value || 0);
    },
  },
};
</script>

<style scoped>
/* Optional custom styles */
</style>
