<template>
  <div class="main-login">
    <div>
      <div class="login login-width login-mobile">
        <h3 class="title text-center text-[#011A51] font-semibold">Sự kiện</h3>

        <div class="search-box d-flex items-center gap-10" >
          <el-input
            v-model="search"
            placeholder="Search"
            prefix-icon="el-icon-search">
          </el-input>
          <img @click="handleRouter('/qr-scan')" src="~/assets/images/icons/qr-scan.svg" alt="">
        </div>

        <div class="main-content-event">
          <el-card v-for="(item,key) in listEvent" :key="key"  :body-style="{ padding: '10px' }" class="card-item mb-10 event" >
          <div class="event-item">
            <el-timeline>
              <div class="event-title d-flex justify-between items-center cursor-pointer"  @click="handleRouter('event/detail/' + item.eventId)">
                <div class="d-flex items-center event-name">
                  <el-badge is-dot class="event-status item" :type="item.eventStatus === 0 ?  '' : 'success'">
                    <ShowAvatarElement :event="{ name: item.eventName, color: item.color }"></ShowAvatarElement>
                  </el-badge>
                  <div class="event-content">
                    <h4 class="title text-bold">{{ item.eventName }}</h4>
                  </div>
                  <i style="margin-left: -5px" class="el-icon event-navi el-icon-arrow-right"></i>

                </div>
                <div class="d-flex items-center ">
                  <span class="text-bold" >{{item.receiptCount}} </span>
                  <i style="margin-left: 3px" class=" el-icon-document"></i>
                </div>
              </div>
              <el-timeline-item placement="top" v-if="item.debt.totalPeople !== 0">
                <el-card>
                  <span class="text-normal-sm">Bạn nợ {{ item.debt.totalPeople }} người khác</span>
                  <span class="text-red"> {{ item.debt.money.amountFormat }}</span>
                </el-card>
              </el-timeline-item>
              <el-timeline-item placement="top" v-if="item.receive.totalPeople !== 0">
                <el-card>
                  <span class="text-normal-sm">{{ item.receive.totalPeople }} người khác nợ bạn </span>
                  <span class="text-green"> {{ item.receive.money.amountFormat }}</span>
                </el-card>
              </el-timeline-item>
            </el-timeline>
            <div>
            </div>
          </div>
        </el-card>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
// import { AUTH_REGISTER, INDEX_SET_ERROR, INDEX_SET_LOADING, INDEX_SET_SUCCESS, SET_EMAIL } from '@/store/store.const'
// import { TYPE_REGISTER_OTP } from '@/store/store.const.js'
// import { validPhoneNoPrefix } from '@/utils/validate'
import { GET_EVENT_LIST, INDEX_SET_LOADING } from '~/store/store.const'

export default {
  name: 'MainPage',
  // middleware: 'auth',
  components: {
    // ShowAvatarElement
  },
  data() {
    return {
      search: '',
      listEvent: []
    }
  },
  created() {
    this.getListEvent()
  },
  mounted() {
  },
  methods: {
    async getListEvent() {
      this.$store.commit(INDEX_SET_LOADING, true)
      try {
        const response = await this.$store.dispatch(GET_EVENT_LIST)
        const { data, statusCode } = response
        if (statusCode === 202) {
          this.listEvent = data
        }
      } catch (e) {
        this.$store.commit(INDEX_SET_LOADING, false)
      }
      this.$store.commit(INDEX_SET_LOADING, false)

      this.listEvent.forEach(element => {
        element.icon_fake = require('@/assets/images/event.png')
      })
    },
    handleRouter(router) {
      this.$router.push(router)
    }
  }
}
</script>
