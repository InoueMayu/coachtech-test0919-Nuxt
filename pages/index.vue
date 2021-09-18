<template>
  <div id="app">
    <h1>独り言App</h1>
    <div class="new">
      <h2>つぶやく内容を入力してください</h2>
      <div class="tweet">
        <label for="tweet"></label>
        <input type="text" name="tweet" id="tweet" v-model="newTweet" />
        <button @click="insertTweet">つぶやく</button>
      </div>
    </div>


    <div class="table">
      <h2>つぶやき一覧</h2>
      <div class="table__border">
      <table>
        <tr v-for="item in tweetLists" :key="item.id">
          <td><div class="icon"></div></td>
          <td >{{item.tweet}}</td>
          <td>
           <button class="delete" @click="deleteTweet(item.id)">削除</button>
          </td>
        </tr>
      </table>
    </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      newTweet: "",
      tweetLists: [],
    };
  },
  methods: {
    async getTweet() {
      const resData = await this.$axios.get(
        "http://127.0.0.1:8000/api/tweet/"
      );
      this.tweetLists = resData.data.data;
    },
    async insertTweet() {
      const sendData = {
        tweet: this.newTweet,
      };
      await this.$axios.post("http://127.0.0.1:8000/api/tweet/", sendData);
      this.getTweet();
    },
    async updateTweet(id, tweet) {
      const sendData = {
        tweet: tweet
      };
      await this.$axios.put(
        "http://127.0.0.1:8000/api/tweet/" + id,
        sendData
      );
      this.getTweet();
    },
    async deleteTweet(id) {
      await this.$axios.delete("http://127.0.0.1:8000/api/tweet/" + id);
      this.getTweet();
    },
  },
  created() {
    this.getTweet();
  },
};
</script>


<style>
#app h1 {
  text-align: center;
}

.new h2 {
  text-align: center;
}

.tweet {
  text-align: center;
}

.table__border {
  border: 1px solid black;
  width: 30%;
  padding: 20px;
  margin: 0 auto;
}

.table h2 {
  text-align: center;
}

.icon {
  background: red;
  width: 50px;
  height: 50px;
  border-radius: 50%;
}

.delete {
  margin-left: 100px;
}

td,
th {
  padding: 5px;
}
</style>
