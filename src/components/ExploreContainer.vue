<template>
  <div id="container">
    <strong>{{ name }}</strong>
    <p>
      Explore
      <a
        target="_blank"
        rel="noopener noreferrer"
        href="https://ionicframework.com/docs/components"
        >UI Components</a
      >
    </p>
    <ion-button @click="login">エミュレーターAuthテスト</ion-button>
    <ion-button @click="dataSet">エミュレーターFirestoreテスト</ion-button>
    <ion-button @click="upload">エミュレーターStorageテスト</ion-button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { IonButton } from '@ionic/vue';
import { createUserWithEmailAndPassword } from 'firebase/auth';
import { doc, setDoc } from 'firebase/firestore';
import { ref, uploadString } from 'firebase/storage';
import { auth, storage, db } from '../main';

export default defineComponent({
  name: 'ExploreContainer',
  props: {
    name: String,
  },
  components: { IonButton },
  setup() {
    async function upload() {
      const storageRef = ref(storage, 'some-child');
      const message = 'This is my message.';
      uploadString(storageRef, message).then((snapshot) => {
        console.log('Uploaded a raw string!', snapshot);
      });
    }

    async function login() {
      createUserWithEmailAndPassword(
        auth,
        'k.matsumoto+01@opening-line.co.jp',
        'password'
      )
        .then((userCredential) => {
          console.log(userCredential, 'success');
        })
        .catch((error) => {
          console.log(error, 'error');
        });
    }

    async function dataSet() {
      await setDoc(doc(db, 'cities', 'LA'), {
        name: 'Los Angeles',
        state: 'CA',
        country: 'USA',
      });
    }
    return {
      dataSet,
      login,
      upload,
    };
  },
});
</script>

<style scoped>
#container {
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  color: #8c8c8c;
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
