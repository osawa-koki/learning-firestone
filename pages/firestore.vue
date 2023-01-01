<template>
  <div>
    <HeaderDiv />
    <main>
      <h2>👍 Cloud Firestoreを使ってみる 👍</h2>
      <div id="InsertContent">
        <div>
          <label for="name" class="form-label">Email your name</label>
          <input id="name" type="text" class="form-control" placeholder="osawa-koki">
        </div>
        <div>
          <label for="text" class="form-label">Content</label>
          <textarea id="text" class="form-control" rows="3"></textarea>
        </div>
        <div class="center box">
          <button type="button" class="btn btn-outline-primary" @click="Insert">Send 🛫🛫🛫</button>
        </div>
      </div>
      <hr />
      <div id="MessagesContent">
        <div class="center box">
          <button type="button" class="btn btn-outline-info" @click="Read">Read 💫💫💫</button>
        </div>
        <ul>
          <li v-for="message in messages" :key="message.id">
            <div>{{ message.name }}</div>
            <div>{{ message.text }}</div>
            <div>{{ message.date }}</div>
          </li>
        </ul>
      </div>
    </main>
    <FooterDiv />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { initializeApp } from 'firebase/app';
import { getFirestore, collection, getDocs, doc, setDoc } from 'firebase/firestore/lite';

import firebaseConfig from '~/firebaseConfig';

import pages from '~/pages';

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

type Message = {
  id: string;
  name: string;
  text: string;
  date: Date;
};

export default defineComponent({
  name: 'FirestorePage',
  data() {
    return {
      pages,
      name: 'osawa-koki',
      text: 'Hello Firestore 💓',
      messages: [] as Message[],
    }
  },
  methods: {
    async Read() {
      const messages_collection = collection(db, 'messages');
      const messages_docs = await getDocs(messages_collection);
      const message_doc = messages_docs.docs.map(doc => doc.data());
      this.messages = message_doc as Message[];
    },
    async Insert() {
      const randomString = Math.random().toString(32).substring(2);
      await setDoc(doc(db, "messages", randomString), {
        id: randomString,
        name: this.name,
        text: this.text,
        date: new Date(),
      });
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
