<template>
  <div class="modal__popup">
    <div class="modal__popup-mask"></div>
    <div data-cy="modal-delete" class="modal__popup-content" :class="{'modal__popup-content_show': initialShow}">
      <div data-cy="modal-delete-icon" class="modal__popup-content-warning">
        <RedWarningVue></RedWarningVue>
      </div>
      <div class="modal__popup-content-text-container">
        <p data-cy="modal-delete-title" class="modal__popup-content-text">Apakah anda yakin ingin menghapus activity <span class="modal__popup-content-text_bold">“{{activityName}}”</span>?</p>
      </div>
      <div class="modal__popup-content-button-container">
        <div data-cy="modal-delete-cancel-button" class="modal__popup-content-button">
          <p class="modal__popup-content-button-text">Batal</p>
        </div>
        <div data-cy="modal-delete-confirm-button" class="modal__popup-content-button modal__popup-content-button_delete">
          <p class="modal__popup-content-button-text modal__popup-content-button-text_delete">Hapus</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import RedWarningVue from '@/assets/images/RedWarning.vue';

export default {
  props: {
    activityName: String
  },

  components: {
      RedWarningVue
  },
  
  data() {
    return {
      initialShow: false
    }
  },

  mounted() {
    setTimeout(() => {
      this.initialShow = true
    },)
  }
}

</script>

<style lang="scss">
  .modal {
    &__popup {
      position: absolute;
      top: 0;
      left: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 100%;
      height: 100%;
      &-mask {
        position: absolute;
        width: 100%;
        height: 100%;
        background-color: #000;
        top: 0;
        left: 0;
        opacity: 0.5;
      }
      &-content {
        &_show {
          top: 0;
          opacity: 1;
        }
        position: relative;
        transition: all 0.20s linear;
        top: -15px;
        opacity: 0;
        padding: 50px 85px 43px 85px;
        background-color: #FFFFFF;
        z-index: 2;
        border-radius: 12px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        &-warning {
          padding: 10px 10px 17px 10px;
          margin-bottom: 34px;
        }
        &-text {
          &-container {
            margin-bottom: 46px;
          }
          font-weight: 500;
          font-size: 18px;
          line-height: 27px;
          color: #111111;
          text-align: center;
          &_bold {
            font-weight: 700;
          }
        }
        &-button {
          &-container {
            display: flex;
          }
          cursor: pointer;
          width: 150px;
          height: 54px;
          background: #F4F4F4;
          border-radius: 45px;
          margin-right: 10px;
          display: flex;
          align-items: center;
          justify-content: center;
          &_delete {
            margin-right: 0px;
            margin-left: 10px;
            background-color: #ED4C5C;
          }
          &-text {
            font-weight: 600;
            font-size: 18px;
            line-height: 27px;
            color: #4A4A4A;
            &_delete {
              color: #FFFFFF;
            }
          }
        }
      }
    }
  }
</style>