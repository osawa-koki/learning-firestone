<template>
  <div>
    <HeaderDiv />
    <main>
      <h2>👍 Cloud Firestoreを使ってみる 👍</h2>
      <div id="InsertContent">
        <div>
          <label for="name" class="form-label">Email your name</label>
          <input id="name" v-model="name" type="text" class="form-control" placeholder="osawa-koki" />
        </div>
        <div>
          <label for="text" class="form-label">Content</label>
          <textarea id="text" v-model="text" class="form-control" rows="3"></textarea>
        </div>
        <div v-if="InsertError !== null" class="alert alert-danger" role="alert">
          {{ InsertError }}
        </div>
        <div class="center box">
          <button v-if="sending === false" type="button" class="btn btn-outline-primary" @click="Insert">Send 🛫🛫🛫</button>
          <button v-else type="button" class="btn btn-outline-primary" disabled>
            <span class="spinner-border spinner-border-sm text-primary" role="status"></span>&nbsp;Sending
          </button>
        </div>
      </div>
      <hr />
      <div id="MessagesContent">
        <div class="center box">
          <button v-if="reading === false" type="button" class="btn btn-outline-info" @click="Read">Read 💫💫💫</button>
          <button v-else type="button" class="btn btn-outline-info" disabled>
            <span class="spinner-border spinner-border-sm text-primary" role="status"></span>&nbsp;Rending
          </button>
        </div>
        <div id="Messages">
          <template v-for="message in messages">
          <!-- eslint-disable-next-line vue/require-v-for-key -->
          <div class="name">{{ message.name }}</div>
          <!-- eslint-disable-next-line vue/require-v-for-key -->
          <div class="text">{{ message.text }}</div>
          <!-- eslint-disable-next-line vue/require-v-for-key -->
          <div class="date">{{ Date2String(message.date) }}</div>
        </template>
        </div>
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
      InsertError: null as string | null,
      sending : false,
      reading : false,
      pages,
      name: 'osawa-koki',
      text: 'Hello Firestore 💓',
      messages: [{
        id: '',
        name: 'osawa-koki',
        text: 'hello-world',
        date: new Date(),
      }] as Message[],
    }
  },
  computed: {
    Date2String() {
      return (date: Date) => {
        try {
          return date.toLocaleString();
        } catch (error) {
          return '????/??/?? ??:??:??';
        }
      }
    }
  },
  methods: {
    async Read() {
      this.reading = true;
      const messages_collection = collection(db, 'messages');
      const messages_docs = await getDocs(messages_collection);
      const message_doc = messages_docs.docs.map(doc => {
        return {
          id: doc.id,
          name: doc.data().name,
          text: doc.data().text,
          date: doc.data().date?.toDate(),
        }
      }).sort((a, b) => {
        if (a.date === undefined) return 1;
        if (b.date === undefined) return -1;
        if (a.date < b.date) return 1;
        if (a.date > b.date) return -1;
        return 0;
      });
      this.messages = message_doc as Message[];
      this.reading = false;
    },
    async Insert() {
      if (this.name === '') {
        this.InsertError = '名前を入力してください';
        return;
      }
      if (this.text === '') {
        this.InsertError = '内容を入力してください';
        return;
      }
      this.text = '';
      this.InsertError = null;
      const randomString = Math.random().toString(32).substring(2);
      this.sending = true;
      await new Promise(resolve => setTimeout(resolve, 1000));
      await setDoc(doc(db, "messages", randomString), {
        id: randomString,
        name: this.name,
        text: this.text,
        date: new Date(),
      });
      this.sending = false;
    }
  },
})
</script>

<style lang="scss" scoped>
#Messages {
  margin-top: 1rem;
  display: grid;
  grid-template-columns: 2fr 5fr 2fr;
  gap: 0.5rem;
  div {
    border: 1px solid rgb(245, 245, 245);
    padding: 0.5rem;
  }
  .date {
    font-size: 0.5rem;
    display: flex;
    align-items: center;
  }
}
</style>
