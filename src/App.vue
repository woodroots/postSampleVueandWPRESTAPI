<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <h1>Nuxt.jsでWPに投稿するやつ</h1>
      </div>
    </div>

    <div class="row mb-2">
      <div class="col-4">コメント</div>
      <div class="col-8">
        <textarea class="form-control" v-model="content"></textarea>
      </div>
    </div>
    <div class="row mb-2">
      <div class="col-4">種類</div>
      <div class="col-8">
        <div class="form-check form-check-inline">
          <input
            class="form-check-input"
            type="radio"
            name="type"
            id="type1"
            value="人間"
            v-model="genre"
          />
          <label class="form-check-label" for="type1">人間</label>
        </div>
        <div class="form-check form-check-inline">
          <input
            class="form-check-input"
            type="radio"
            name="type"
            id="type2"
            value="犬"
            v-model="genre"
          />
          <label class="form-check-label" for="type2">犬</label>
        </div>
        <div class="form-check form-check-inline">
          <input
            class="form-check-input"
            type="radio"
            name="type"
            id="type3"
            value="人外"
            v-model="genre"
          />
          <label class="form-check-label" for="type3">人外</label>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <button class="btn btn-primary" v-on:click="post">投稿する</button>
      </div>
    </div>

    <div class="row border-top pt-4 mt-4 align-items-stretch">
      <div class="col-4 mb-4" v-for="(item,index) in posts" :key="index">
        <div class="p-2 border h-100">
          <div v-html="item.content.rendered"></div>
          <div
            class="text-right small"
            v-if="item.custom_fields.genre"
          >{{ item.custom_fields.genre[0] }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      content: "",
      genre: "",
      posts: []
    };
  },
  mounted() {
    var vm = this;
    vm.url = "http://posted.local/wp-json/wp/v2";

    vm.get();
  },
  methods: {
    post() {
      var vm = this;
      axios
        .post(
          vm.url + "/posts",
          {
            content: vm.content,
            author: 1,
            meta: {
              genre: vm.genre
            },
            status: "publish"
          },
          {
            headers: {
              Authorization:
                "Basic YWRtaW46cHE4eSBRV2szIDhaREYgbFJrcyBlOFpxIGxnc3M="
            }
          }
        )
        .then(function() {
          vm.get();
        });
    },
    get() {
      var vm = this;
      axios.get(vm.url + "/posts").then(function(response) {
        vm.posts = response.data;
      });
    }
  }
};
</script>

<style>
</style>
