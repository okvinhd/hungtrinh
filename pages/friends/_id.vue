<template>
  <div class="main-login">
    <div class="search d-flex justify-between items-center mb-5">
      <div>Search Bar Here</div>
      <div>
        <img src="~/assets/images/common/add-friend.svg" alt="" class="cursor-pointer" @click="handleGoToNewFriends"/>
      </div>
    </div>
    <div class="tab d-flex justify-around">
      <div
        class="text-[#848484] font-semibold pb-1 cursor-pointer"
        :class="{ 'tab-selected': selectedTab === 'friendList' }"
        @click="handleChangeTab('friendList')"
      >
        {{ $t('friends.friends_list') }}
      </div>
      <div
        class="text-[#848484] font-semibold pb-1 cursor-pointer"
        :class="{ 'tab-selected': selectedTab === 'friendRequest' }"
      >
        <el-badge :value="12" class="item">
          <span @click="handleChangeTab('friendRequest')">{{
            $t('friends.friends_request')
          }}</span>
        </el-badge>
      </div>
    </div>
    <CurrentFriendTab
      v-show="selectedTab === 'friendList'"
      :list-friend="listFriend"
      @reload="getListFriend"
    ></CurrentFriendTab>
    <RequestFriendTab
      v-show="selectedTab === 'friendRequest'"
      :list-friend="listRequestFriend"
      @reload="getListRequest"
    ></RequestFriendTab>
  </div>
</template>

<script>
import CurrentFriendTab from '@/components/friends/CurrentFriendTab.vue'
import RequestFriendTab from '@/components/friends/RequestFriendTab.vue'
import { FRIEND_LIST, FRIEND_LIST_REQUEST, INDEX_SET_LOADING } from '~/store/store.const'
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Friends',
  components: {
    CurrentFriendTab,
    RequestFriendTab
  },
  data() {
    return {
      selectedTab: 'friendList',
      listFriend: [],
      listRequestFriend: []
    }
  },
  created() {
    this.getListFriend()
    this.getListRequest()
  },
  methods: {
    handleChangeTab(tab) {
      this.selectedTab = tab
    },
    handleGoToNewFriends() {
      this.$router.push('/friends/new')
    },
    async getListFriend() {
      this.$store.commit(INDEX_SET_LOADING, true)
      try {
        const response = await this.$store.dispatch(FRIEND_LIST)
        const { data, statusCode } = response
        if (statusCode === 202) {
          this.listFriend = data
        }
      } catch (e) {
        this.$store.commit(INDEX_SET_LOADING, false)
      }
      this.$store.commit(INDEX_SET_LOADING, false)
    },
    async getListRequest() {
      this.$store.commit(INDEX_SET_LOADING, true)
      try {
        const response = await this.$store.dispatch(FRIEND_LIST_REQUEST)
        const { data, statusCode } = response
        if (statusCode === 202) {
          this.listRequestFriend = data
        }
      } catch (e) {
        this.$store.commit(INDEX_SET_LOADING, false)
      }
      this.$store.commit(INDEX_SET_LOADING, false)
    }
  }
}
</script>

<style lang="scss">
.tab {
  border-bottom: 3px solid transparent;
  &-selected {
    color: #000000;
    border-bottom: 3px solid #28a8d7;
  }
}
</style>
