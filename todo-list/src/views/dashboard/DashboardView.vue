<template>
  <div class="dashboard__container">
    <div class="dashboard__header">
      <p data-cy="activity-title" class="dashboard__header-text">Activity</p>
      <div class="dashboard__header-button">
        <div data-cy="activity-add-button" class="dashboard__header-button-shape">
          <Plus></Plus>
          <p class="dashboard__header-button-shape-text">Tambah</p>
        </div>
      </div>
    </div>
    <div class="dashboard__empty-state">
      <ActivityEmptyStateVue></ActivityEmptyStateVue>
    </div>
  </div>
</template>

<script>
import Plus from "../../assets/images/PlusIcon.vue"
import ActivityEmptyStateVue from "@/components/ActivityEmptyState.vue";

export default {
  name: 'DashboardView',
  components: {
    Plus,
    ActivityEmptyStateVue
  },
  data() {
    return {
    }
  }
}

</script>
