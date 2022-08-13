<template>
  <div data-cy="activity-empty-state" class="activity__empty-state">
    <PersonEmptyStateVue></PersonEmptyStateVue>
  </div>
</template>

<script>

import PersonEmptyStateVue from '@/assets/images/ActivityEmptyState.vue';

export default {
  name: 'ActivityEmptyState',
  components: {
    PersonEmptyStateVue
  },
  data() {
    return {
    }
  }
}

</script>