<template>
  <h2>{{title}}</h2>
  <ul>
    <li v-for="item in list" key="item.id">
      <span @click="markItem(itemDone, 2)">
        <img src={{imgSrc}} />
        {{ item }}
      </span>
      <i @click="deleteItem(itemDone, 2)">✘</i>
    </li>
  </ul>
</template>

<script>
export default {
    props: {
        title: String,
        list: {},
        imgSrc: String,
    },
};
</script>

<style scoped>

</style>