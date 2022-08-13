<template>
  <div class="activity-detail__container">
    <div class="activity-detail__header">
      <div class="activity-detail__header-container">
        <div class="activity-detail__header-back" @click="backToDashboard()">
          <ChevronLeftVue></ChevronLeftVue>
        </div>
        <input data-cy="activity-title" class="activity-detail__header-text" :class="{'activity-detail__header-text_focused': inputFocused}" v-model="inputValue" v-show="inputFocused === true" @blur="setInputFocus(false)">
        <p data-cy="activity-title" class="activity-detail__header-text" @click="setInputFocus(true)" v-show="inputFocused === false">{{inputValue}}</p>
        <EditIcon></EditIcon>
      </div>
      <div class="activity-detail__header-button">
        <div data-cy="activity-add-button" class="activity-detail__header-button-shape">
          <Plus></Plus>
          <p class="activity-detail__header-button-shape-text" @click="createNewActivity()">Tambah</p>
        </div>
      </div>
    </div>
    <div data-cy="todo-empty-state" class="activity-detail__empty-state">
      <div class="activity-detail__list-item-container" v-if="listItem?.todo_items?.length > 0">
        <div class="activity-detail__list-item" v-for="(dt,index) in listItem.todo_items" :key="index">
          <div class="activity-detail__list-item-content">
            <input type="checkbox" class="activity-detail__list-item-content-checkbox" :class="{'activity-detail__list-item-checkbox_checked': !dt.is_active}" v-model="dt.is_active" :true-value="false" :false-value="true"/>
            <StatusView :status="dt.priority"></StatusView>
            <p class="activity-detail__list-item-content-title">{{dt.title}}</p>
            <div class="activity-detail__list-item-content-edit">
              <EditIcon></EditIcon>
            </div>
            <div class="activity-detail__list-item-content-delete">
              <DeleteButtonVue></DeleteButtonVue>
            </div>
          </div>
        </div>
      </div>
      <ActivityEmptyStateVue :imageNumber="2" v-else></ActivityEmptyStateVue>
    </div>

    <!-- Modal -->
    <PopupView v-if="deletePopup" :activityName="'New Activity'" @close-modal="eventModalClossed" @get-list-card="getListCardData" :activityId="selectedId"></PopupView>
  </div>
</template>

<script>
import Plus from "../../assets/images/PlusIcon.vue"
import EditIcon from "../../assets/images/EditIcon.vue";
import DeleteButtonVue from "@/assets/images/DeleteButton.vue";
import ActivityEmptyStateVue from "@/components/ActivityEmptyState.vue";
import PopupView from "@/components/PopupView.vue";
import ChevronLeftVue from "@/components/ChevronLeft.vue";
import StatusView from "@/components/StatusView.vue";

import ClickOutside from 'vue-click-outside'

export default {
  name: 'ActivityView',
  components: {
    Plus,
    ActivityEmptyStateVue,
    PopupView,
    ChevronLeftVue,
    EditIcon,
    DeleteButtonVue,
    StatusView
  },

  data() {
    return {
      listItem: [],
      selectedId: 0,
      deletePopup: false,
      inputFocused: false,
      inputValue: "-"
    }
  },
  mounted() {
    this.getListItem()
  },
  methods: {
    getListItem() {
      this.$http.get("https://todo.api.devcode.gethired.id/activity-groups/"+this.$route.params.id).then((response) => {
        this.listItem = response.data
        this.inputValue = this.listItem.title
      }, err => {
        console.log(err)
      })
    },
    updateTitle() {
      this.$http.patch("https://todo.api.devcode.gethired.id/activity-groups/"+this.$route.params.id, {title: this.inputValue ? this.inputValue : "-"}).then((response) => {
        this.listItem = {
          ...this.listItem,
          title: response.data.title
        }
      }, err => {
        console.log(err)
      })
    },
    createNewItem() {
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
    },
    setInputFocus(param) {
      this.inputFocused = param
      const elementInput = document.getElementsByClassName("activity-detail__header-text")[0]
      setTimeout(() => {
        elementInput.focus()
      })

      if(param === false) {
        this.updateTitle()
      }
    },
    backToDashboard() {
      this.$router.push("/")
    }
  },

  directives: {
    ClickOutside
  },
  
}

</script>
