<template>
  <div class="container">
    <div>
      <Logo/>
      <h1 class="title">
        {{title}}
      </h1>
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
            console.log('abcd', doc.data());
            this.dataSet.push(doc.data());
          });
        });
      } catch (e) {
        alert(e);
      }
    }
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
