<template>
  <div class="container">
    <!-- 참여중인 챌린지 : {{ this.challenge_groupList.length }} -->

    <div class="container m-5">
      <div class="row">
        <div class="col-4">
          <div
            class="row"
            v-for="(challenge, index) in challenge_groupList"
            :key="index"
          >
            <div class="col-12 p-2">
              <div class="card p-2 wow fadeInUp" data-wow-delay="0.1s">
                <div class="card-body">
                  <h5 class="card-title">{{ challenge.challenge_title }}</h5>
                  <span class="card-text"
                    >모임장 : {{ challenge.challenge_makeUser_nickname }}</span
                  ><br />
                  |
                  <span class="card-text"
                    >지역 : {{ challenge.challenge_location }}</span
                  >
                  |
                  <p class="card-text">
                    진행기간 : {{ challenge.challenge_startDate }} ~
                    {{ challenge.challenge_endDate }}
                  </p>
                  <a
                    class="btnJoin btn btn-primary"
                    @click="showForm(challenge.challenge_id)"
                    >대화하기</a
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col">
          <div v-if="showMessenger" class="col-12 p-2">
                  <challenge-chatting
                    :id="parseInt(id_for_callMsg)"
                  ></challenge-chatting>
          </div>
          <div v-else class="col-12 p-2 m-2 animated zoomIn" style="height:745px;">
            <img src="../../assets/Creative Chatting App Logo2.png">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapGetters } from "vuex";
import ChallengeChatting from "./ChallengeChatting.vue";

export default {
  name: "MyChallenge",
  components: {
    ChallengeChatting,
  },
  data() {
    return {
      showMessenger: false,
      id_for_callMsg: 0,
      message_content: "",
    };
  },

  methods: {
    showForm(challenge_id) {
      this.showMessenger = true;
      this.id_for_callMsg = challenge_id;
      //  this.$refs.change.id = challenge_id;
    },
    registMsg() {
      if (this.message_content === "") {
        alert("내용을 입력해주세요");
        return;
      }

      const message = {
        challenge_id: this.id_for_callMsg,
        message_content: this.message_content,
        message_writer_id: this.loginToken,
      };
      this.$store.dispatch("registMessage", message);
      this.message_content = "";
    },
    deleteMsg(msg) {
      this.$store.dispatch("deleteMessage", msg);
    },
  },

  watch: {
    id_for_callMsg: function (val) {
      this.$store.dispatch("setMessages", val);
    },
  },

  computed: {
    ...mapState(["MyChallenges", "loginToken", "messages", "loginUserId"]),
    ...mapGetters(["challenge_groupList"]),
  },
};
</script>

<style scoped>
.btnJoin {
  width: 100px;
  height: 40px;
}
.btnmsg {
  width: 30px;
  height: 30px;
}
</style>
