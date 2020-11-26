<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">
        Admin
      </div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong> {{ followers }}
      </div>

      <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot">
        <label for="newTwoot"><strong>New Twoot</strong></label>
        <textarea id="newTwoot" rows="4" v-model="newTwootContent"></textarea>
        <div class="user-profile__create-twoot-type">
          <label for="newTwootType"><strong>Type</strong></label>
          <select id="newTwootType" v-model="selectedTwootType">
            <option
              :value="option.value"
              v-for="(option, index) in twootType"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>
        </div>

        <button>Twoot!</button>
      </form>
    </div>

    <div class="user-profile__twoots-wrapper">
      <TwootItem
        class="user-profile__twoot"
        v-for="twoot in user.twoots"
        :key="twoot.id"
        :username="user.username"
        :twoot="twoot"
        @favorite="toggleFavorite"
      />
    </div>
  </div>
</template>

<script>
import TwootItem from "./TwootItem";
export default {
  name: "UserProfile",
  components: { TwootItem },
  data() {
    return {
      followers: 0,
      twootType: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Twoot" },
      ],
      newTwootContent: "",
      selectedTwootType: "instant",
      user: {
        id: 1,
        username: "3xbun",
        firstName: "Bunnasorn",
        lastName: "Kaewsiri",
        email: "bunnasorn.k@ku.th",
        isAdmin: true,
        twoots: [
          {
            id: 1,
            content:
              "Poison ivy grew through the fence they said was impenetrable.",
          },
          {
            id: 2,
            content:
              "I purchased a baby clown from the Russian terrorist black market.",
          },
          {
            id: 3,
            content:
              "The waitress was not amused when he ordered green eggs and ham.",
          },
        ],
      },
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowersCount) {
      if (oldFollowersCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a followers!`);
      }
    },
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
  },
  methods: {
    toggleFavorite(id) {
      console.log(`Favorited Twoot #${id}`);
    },
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== "draft") {
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent,
        });
      }

      this.newTwootContent = "";
    },
  },
};
</script>

<style>
h1 {
  margin: 0;
}

.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;
}

.user-profile__user-panel {
  display: flex;
  flex-direction: column;
  margin-right: 50px;
  padding: 20px;
  background-color: #fff;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
}

.user-profile__admin-badge {
  background: purple;
  color: white;
  border-radius: 5px;
  margin-right: auto;
  padding: 0 10px;
  font-weight: bold;
  margin: 0.5em auto 0.5em 0;
}

.user-profile__create-twoot {
  display: flex;
  padding-top: 20px;
  flex-direction: column;
}

textarea {
  resize: none;
}
</style>
