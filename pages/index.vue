<template>
  <div class="container">
    <div>
      <Logo/>
      <h1 class="title">
        {{title}}
      </h1>
      <div class="d-flex flex-row justify-content-center w-100">
        <div v-for="(msg, index) in dataSet" :key="index" class="d-flex flex-column bg-primary text-white mx-2  w-25">
          <span>{{msg.text}} - {{new Date(msg.createdat.seconds * 1000)}}</span>
          <b-button @click="set(msg)">update</b-button>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import Vue from 'vue'

export default Vue.extend({
  data: () => {
    return {
      title: 'dungon crawler',
      dataSet: []
    };
  },
  beforeMount() {
    this.readFromFirestore();
  },
  methods: {
    async readFromFirestore() {
      const messageRef = this.$fire.firestore.collection('messages');
      try {
        messageRef.get().then(res => {
          res.forEach(doc => {
            const data = doc.data();
            data.id = doc.id;
            console.log('abcd', data);
            this.dataSet.push(data);
          });
        });
      } catch (e) {
        alert(e);
      }
    },
    async add() {
      const messageRef = this.$fire.firestore.collection('messages');
      try {
        messageRef.add({text: 'lol', createdat: new Date()}).then(res => {
          console.log('abcd', res);
        });
      } catch (e) {
        alert(e);
      }
    },
    async set(msg) {
      const data = {...msg};
      data.createdat = new Date();
      const messageRef = this.$fire.firestore.collection('messages').doc(data.id);
      try {
        await messageRef.set(data);
      } catch (e) {
        alert(e);
      }
    },
  }
})
</script>

<style lang="scss">
  .container {
    margin: 0 auto;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }

  .title {
    font-family: 'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
    display: block;
    font-weight: 300;
    font-size: 100px;
    color: #35495e;
    letter-spacing: 1px;
  }
</style>
