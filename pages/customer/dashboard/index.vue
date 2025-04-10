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
              <h5>
                <i class="fa fa-tachometer-alt"></i> DASHBOARD
              </h5>
              <hr />

              <!-- Welcome Alert -->
              <div class="alert alert-success" role="alert">
                Selamat Datang Paduka <strong>{{ $auth.user.name }}</strong>
              </div>

              <!-- Statistik Boxes -->
              <div class="row">
                <StatBox
                  color="primary"
                  icon="fa-circle-notch fa-spin"
                  label="PENDING"
                  :value="pending"
                />
                <StatBox
                  color="success"
                  icon="fa-check-circle"
                  label="SUCCESS"
                  :value="success"
                />
                <StatBox
                  color="warning"
                  icon="fa-exclamation-triangle"
                  label="EXPIRED"
                  :value="expired"
                />
                <StatBox
                  color="danger"
                  icon="fa-times-circle"
                  label="FAILED"
                  :value="failed"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// Import Sidebar
import Sidebar from "@/components/web/sidebar.vue";

// Statistik Box Component (opsional agar DRY)
const StatBox = {
  props: ["color", "icon", "label", "value"],
  template: `
    <div class="col-6 col-lg-3 mb-3">
      <div class="card rounded shadow-sm overflow-hidden">
        <div class="card-body p-0 d-flex align-items-center">
          <div :class="'bg-' + color + ' py-4 px-5 mfe-3'">
            <i :class="'fas ' + icon + ' fa-2x'"></i>
          </div>
          <div>
            <div :class="'text-value text-' + color">{{ value }}</div>
            <div class="text-muted text-uppercase font-weight-bold small">
              {{ label }}
            </div>
          </div>
        </div>
      </div>
    </div>
  `,
};

export default {
  layout: "default",
  middleware: "isCustomer",

  components: {
    Sidebar,
    StatBox,
  },

  head() {
    return {
      title: "Dashboard - Customer",
    };
  },

  async asyncData({ $axios, error }) {
    try {
      const dashboard = await $axios.$get("/api/customer/dashboard");

      return {
        pending: dashboard.data.count.pending,
        success: dashboard.data.count.success,
        expired: dashboard.data.count.expired,
        failed: dashboard.data.count.failed,
      };
    } catch (err) {
      // Optional error handler / default value
      return {
        pending: 0,
        success: 0,
        expired: 0,
        failed: 0,
      };
    }
  },
};
</script>

<style scoped>
/* Add any needed styling here */
</style>
