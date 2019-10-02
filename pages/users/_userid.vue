<template>
  <section class="container">
    <h3>{{ user.id }}</h3>
    <img class="avator" :src="user.profile_image_url" width="96" alt />
    <p>{{ user.description || 'No description' }}</p>
    <p>
      <nuxt-link to="/">
        <small>
          <b>トップへ戻る</b>
        </small>
      </nuxt-link>
    </p>
    <h3>{{ user.id }} さんの投稿一覧</h3>
    <ul>
      <li v-for="item in items" :key="item.id">
        <h4>
          <span>{{ item.title }}</span>
        </h4>
        <div>{{ item.body.slice(0, 130) }}......</div>
        <p>
          <a :href="item.url">{{ item.url }}</a>
        </p>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  head() {
    return {
      title: this.user.id
    };
  },
  async asyncData({ app, route }) {
    const user = await app.$axios.$get(
      `https://qiita.com/api/v2/users/${route.params.userid}`
    );
    const items = await app.$axios.$get(
      `https://qiita.com/api/v2/items?query=user:${route.params.userid}`
    );
    return {
      user,
      items
    };
  }
};
</script>

<style>
.avator {
  border-radius: 50%;
}

.container {
  min-height: 100vh;
  padding: 16px;
}

h3 {
  margin: 16px 0;
  padding: 8px;
  border-bottom: solid 1px #e5e5e5;
}

li + li {
  margin: 16px 0;
}

p {
  margin: 8px 0;
}
</style>