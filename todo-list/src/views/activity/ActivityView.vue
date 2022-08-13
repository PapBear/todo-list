<template>
  <div class="activity__container">
    <div class="activity__header">
      <p data-cy="activity-title" class="activity__header-text">Activity</p>
      <div class="activity__header-button">
        <div data-cy="activity-add-button" class="activity__header-button-shape">
          <Plus></Plus>
          <p class="activity__header-button-shape-text" @click="createNewActivity()">Tambah</p>
        </div>
      </div>
    </div>
    <div class="activity__empty-state">
      <ActivityEmptyStateVue></ActivityEmptyStateVue>
    </div>

    <!-- Modal -->
    <PopupView v-if="deletePopup" :activityName="'New Activity'" @close-modal="eventModalClossed" @get-list-card="getListCardData" :activityId="selectedId"></PopupView>
  </div>
</template>

<script>
import Plus from "../../assets/images/PlusIcon.vue"
import ActivityEmptyStateVue from "@/components/ActivityEmptyState.vue";
import PopupView from "@/components/PopupView.vue";

export default {
  name: 'ActivityView',
  components: {
    Plus,
    ActivityEmptyStateVue,
    PopupView
  },
  data() {
    return {
      listActivity: [],
      selectedId: 0,
      deletePopup: false,
    }
  },
  mounted() {
    this.getListCardData()
  },
  methods: {
    getListCardData() {
      // const encodeEmail = encodeURIComponent("joshuahendrawan03@gmail.com")

      this.$http.get("https://todo.api.devcode.gethired.id/activity-groups", {params: {email: "joshuahendrawan03@gmail.com"}}).then((response) => {
        this.listActivity = response.data.data
        console.log(this.listActivity)
      }, err => {
        console.log(err)
      })
    },
    createNewActivity() {
      const formData = {
        "title": "New Activity",
        "email": "joshuahendrawan03@gmail.com",
      }
      this.$http.post("https://todo.api.devcode.gethired.id/activity-groups", formData).then(() => {
        this.getListCardData()
      }, err => {
        console.log(err)
      })
    },
    setVisibilityDeletePopup(dt) {
      this.deletePopup = !this.deletePopup
      this.selectedId = dt.id
      console.log(this.selectedId)
    },
    eventModalClossed() {
      setTimeout(() => {
        this.deletePopup = false
      }, 250)
    }
  }
}

</script>
