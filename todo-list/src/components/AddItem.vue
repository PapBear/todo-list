<template>
  <div class="modal__add">
    <div class="modal__add-mask" @click="closeModal()"></div>
    <div class="modal__add-content" :class="{'modal__add-content_show': initialShow}" v-show="state === 'popup'">
      <div class="modal__add-content-header">
        <p class="modal__add-content-header-text">Tambah List Item</p>
        <div class="modal__add-content-header-icon">
          <CloseIcon></CloseIcon>
        </div>
      </div>
      <div class="modal__add-content-body">
        <p class="modal__add-content-body-title">Nama list item</p>
        <input data-cy="activity-title" class="modal__add-content-body-input" :class="{'modal__add-content-body-input_focused': isFocused}" v-model="inputItem" :placeholder="'Tambahkan nama list item'" @focus="isFocused = true" @blur="isFocused = false">

        <p class="modal__add-content-body-title">Priority</p>
        <div class="modal__add-content-body-priority-container">
          <div class="modal__add-content-body-priority" :class="{'modal__add-content-body-priority_opened': openDropdown}" @click="setDropDown()">
            <StatusView :status="selectedPriority.value" :bigger="true" v-if="!openDropdown"></StatusView>
            <p class="modal__add-content-body-priority-text" v-if="!openDropdown">{{selectedPriority.text}}</p>
            <p class="modal__add-content-body-priority-text modal__add-content-body-priority-text_opened" v-else>Pilih priority</p>
            <div class="modal__add-content-body-priority-chevron">
              <ChevronDown v-if="!openDropdown"></ChevronDown>
              <ChevronUp v-else></ChevronUp>
            </div>
          </div>
          <div class="modal__add-content-body-priority-list" v-if="openDropdown">
            <div class="modal__add-content-body-priority-list-card" v-for="(dt, index) in priorityList" :key="index" @click="selectPriority(dt)">
              <StatusView :status="dt.value" :bigger="true"></StatusView>
              <p class="modal__add-content-body-priority-list-card-text">{{dt.text}}</p>
              <div class="modal__add-content-body-priority-list-card-checklist">
                <ChecklistIcon v-if="selectedPriority.value === dt.value"></ChecklistIcon>
              </div>
            </div>
          </div>
        </div>

      </div>
      <div class="modal__add-content-button">
        <div class="modal__add-content-button-shape">
          <p class="modal__add-content-button-shape-text">Simpan</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import ClickOutside from 'vue-click-outside'
import CloseIcon from '../assets/images/CloseIcon.vue'
import StatusView from './StatusView.vue'
import ChevronDown from '../assets/images/ChevronDown.vue'
import ChevronUp from '../assets/images/ChevronUp.vue'
import ChecklistIcon from '../assets/images/ChecklistIcon.vue'

export default {
  props: {

  },

  components: {
    CloseIcon,
    StatusView,
    ChevronDown,
    ChevronUp,
    ChecklistIcon
},
  
  data() {
    return {
      initialShow: false,
      successDeleteModal: false,
      isFocused: false,
      firstLoad: false,
      state: "popup",
      inputItem: "",
      priorityList: [
        {
          text: "Very High",
          value: "very-high"
        },
        {
          text: "High",
          value: "high"
        },
        {
          text: "Normal",
          value: "normal"
        },
        {
          text: "Low",
          value: "low"
        },
        {
          text: "Very Low",
          value: "very-low"
        }
      ],
      selectedPriority: {
        text: "Very High",
        value: "very-high"
      },
      openDropdown: false
    }
  },

  mounted() {
    setTimeout(() => {
      this.initialShow = true
      this.firstLoad = true
    },)
  },

  // do not forget this section
  directives: {
    ClickOutside
  },

  methods: {
    closeModal() {
      if(this.firstLoad) {
        this.initialShow = false
        this.successDeleteModal = false
        this.$emit("close-modal")
      }
    },
    setDropDown() {
      this.openDropdown = !this.openDropdown
    },
    selectPriority(dt) {
      this.selectedPriority = dt
      this.openDropdown = false
    },
    createNewItem() {
      const formData = {
        "activity_group_id": this.$route.params.id,
        "title": this.inputItem,
        "priority": this.selectPriority.value,
      }
      this.$http.post("https://todo.api.devcode.gethired.id/todo-items", formData).then(() => {
        // 
      }, err => {
        console.log(err)
      })
    },
  }
}

</script>

<style lang="scss">
  .modal {
    &__add {
      position: fixed;
      top: 0;
      left: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 100%;
      height: 100%;
      &-mask {
        position: fixed;
        width: 100%;
        height: 100%;
        background-color: #000;
        top: 0;
        left: 0;
        opacity: 0.5;
      }
      &-content {
        position: relative;
        transition: top 0.20s linear, opacity 0.20s linear;
        max-width: 830px;
        top: -30px;
        opacity: 0;
        padding: 24px 0px 19px 0px;
        background-color: #FFFFFF;
        z-index: 2;
        border-radius: 12px;
        display: flex;
        flex-direction: column;
        &_show {
          top: 0;
          opacity: 1;
          width: 100%;
        }
        &-header {
          display: flex;
          align-items: center;
          padding: 0px 47px 19px 30px;
          border-bottom: 1px solid #E5E5E5;
          &-text {
            flex: 0.5;
            font-weight: 600;
            font-size: 18px;
            line-height: 27px;
            color: #111111;
          }
          &-icon {
            cursor: pointer;
            flex: 0.5;
            text-align: end;
          }
        }
        &-body {
          padding: 38px 41px 0px 30px;
          &-input {
            width: 100%;
            height: 52px;
            padding: 0 18px;
            background: #FFFFFF;
            border: 1px solid #E5E5E5;
            border-radius: 6px;
            margin-bottom: 26px;
            margin-top: 9px;
            outline: none;
            &_focused {
              border-color: #16ABF8;
            }
            &::placeholder {
              color: #A4A4A4;
            }
          }
          &-priority {
            cursor: pointer;
            position: relative;
            display: flex;
            align-items: center;
            width: fit-content;
            min-width: 205px;
            padding: 14px 23px 14px 17px;
            background: #FFFFFF;
            border: 1px solid #E5E5E5;
            border-radius: 6px;
            margin-top: 9px;
            &_opened {
              background-color: #E5E5E5;
              border-bottom: none;
              border-bottom-right-radius: 0px;
              border-bottom-left-radius: 0px;
            }
            &-container {
              position: relative;
            }
            &-text {
              font-weight: 400;
              font-size: 16px;
              line-height: 24px;
              color: #111111;
              margin-left: 19px;
              margin-right: 37px;
              &_opened {
                margin-left: 0px;
                margin-right: 56px;
              }
            }
            &-list {
              cursor: pointer;
              position: absolute;
              top: 52px;
              left: 0px;
              &-card {
                display: flex;
                align-items: center;
                width: 205px;
                height: 52px;
                padding: 14px 22px 14px 17px;
                background: #FFFFFF;
                border: 1px solid #E5E5E5;
                border-bottom: none;
                &-text {
                  font-weight: 400;
                  font-size: 16px;
                  line-height: 24px;
                  color: #111111;
                  margin-left: 19px;
                  margin-right: 37px;
                }
                &:last-child {
                  border-radius: 0px 0px 6px 6px;
                  border-bottom: 1px solid #E5E5E5;
                }
                &-checklist {
                  flex: 1;
                  text-align: end;
                }
              }
            }
            &-chevron {
              flex: 1;
              text-align: end;
            }
          }
          &-title {
            font-weight: 600;
            font-size: 12px;
            line-height: 18px;
            color: #111111;
          }
        }
        &-button {
            border-top: 1px solid #E5E5E5;;
            margin-top: 23px;
            padding-top: 15px;
            padding-right: 40px;
            display: flex;
            justify-content: flex-end;
            &-shape {
              display: flex;
              flex-direction: row;
              align-items: center;
              cursor: pointer;
              padding: 13.5px 29px 13.5px 27px;
              background-color: #16ABF8;
              width: fit-content;
              border-radius: 45px;
              &-text {
                font-weight: 600;
                font-size: 18px;
                line-height: 27px;
                color: #FFFFFF;
                margin-left: 6px;
              }
            }
          }
      }
    }
  }
</style>