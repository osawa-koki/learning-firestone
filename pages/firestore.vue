<template>
  <div>
    <HeaderDiv />
    <main>
      <h2>👍 Cloud Firestoreを使ってみる 👍</h2>
    </main>
    <FooterDiv />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import pages from '~/pages';

export default defineComponent({
  name: 'FirestorePage',
  data() {
    return {
      pages,
    }
  },
})
</script>

<style lang="scss" scoped>
.Central {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  h1 {
    margin-top: 1rem;
  }
  img {
    margin-top: 1rem;
    border-radius: 50%;
    border: 1px lightgray solid;
  }
}
#Contents {
  display: flex;
  list-style: none;
  padding: 0;
  li {
    margin-right: 0.5rem;
    margin-bottom: 1rem;
    padding-right: 0.5rem;
    border-right: 1px lightgray solid;
    &:last-child {
      margin-right: 0;
      padding-right: 0;
      border-right: none;
    }
    a {
      font-size: 1.1rem;
      color: #0E6DFE;
      text-decoration: none;
      &:hover {
        text-decoration: underline;
      }
    }
  }
}
</style>
