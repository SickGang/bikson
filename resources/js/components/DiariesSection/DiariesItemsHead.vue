<template>
  <div class="item-section d-flex flex-column flex-md-row align-items-center p-md-5">
    <div class="item-section_link col-lg-8">
      <div class="goods-list_item__img-sale text-left" v-if="item.sale_flag == 1">
        Акция!
      </div>
      <a href="#">
        <img @click="showPopup" :src="item.img_url" alt="">
      </a>
    </div>
    <div class="item-section_info d-flex flex-column col-lg-4 mt-0 mt-sm-3">
      <div class="item-section_info_top">
        <h3>{{item.title}}</h3>
        <span>{{item.price}} ₽</span>
      </div>
      <div class="item-section_info_bottom">
        <div class="item-section_info_bottom-button mt-3 d-flex justify-content-center">
          <tippy class="mb-2" trigger="click" theme="light" interactive="true" zIndex="3" size="large" arrow="true" :duration="[600,600]" :delay="[400,0]">
            <template v-slot:trigger>
              <button @click="addToCart(item)" class="item-button">
                Купить
              </button>
            </template>
            <div class="mb-3">Товар добавлен в корзину</div>
            <a @click="showBasket" class="item-button item-button_mod" href="#">В корзину</a>
          </tippy>
          <button @click="showPopup" class="item-button item-button_mod" >Подробнее</button>
        </div>
      </div>
    </div>
    <transition name="modal-fade">
    <popup-info v-if="isPopupVisible">
      <button type="button" class="close close-modal" data-dismiss="modal" aria-label="Close" @click="closePopup"><span aria-hidden="true">&times;</span></button>
      <div class="popup_title font-weight-bold col-12 font mb-2">
        <h3>{{item.title}}</h3>
      </div>
      <div class="popup_info-wrap row">
        <div class="popup_image col-12 col-md-6">
          <newswiper :itemId="item.id"></newswiper>
        </div>
        <div class="popup_info col-12 col-md-4">
          <div class="popup_info-spec">
            <tbody>
              <tr><td><span>Стоимость:</span></td><td><span class="font-weight-bold">{{item.price}} ₽</span></td></tr>
            </tbody>
            <div class="text-left" v-html="item.description">
            </div>
          </div>
          <div class="popup_info-btn mt-2">
            <a @click="closePopup" class="item-button" href="#">Закрыть</a>
            <a @click="addToCart(item)" class="item-button" href="#">В корзину</a>
          </div>
        </div>
      </div>
    </popup-info>
    </transition>
  </div>
</template>

<script>
import popupInfo from '../PopupInfo.vue'
import newswiper from '../Newswiper.vue'
import tooltip from '../Tooltip.vue'
import {mapGetters} from 'vuex'

export default {
  props: ['item'],
  data() {
    return {
      isPopupVisible: false
    }
  },
  computed: {
    ...mapGetters([
      'SHOW_ADD_ITEM'
    ])
  },
  methods: {
    addToCart(item) {
      event.preventDefault()
      setTimeout(() => {
        this.$children[0].tip.hide()
      }, 3000)
      this.isPopupVisible = false
      this.$store.commit('addToCart', item)
    },
    showBasket() {
      event.preventDefault()
      this.$store.dispatch('TOGGLE_ADD_ITEM')
    },
    showPopup() {
      event.preventDefault()
      this.isPopupVisible = true
    },
    closePopup() {
      event.preventDefault()
      this.isPopupVisible = false
    }
  },
  components: {popupInfo,newswiper,tooltip}
}
</script>

<style lang="css" scoped>
</style>
