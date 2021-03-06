<template>
  <div>
    <ion-toolbar color="light">
      <ion-grid id="home-header">
        <ion-row>
          <ion-col
            size-lg="4"
            class="ion-hide-lg-down librescore-logo"
          ></ion-col>

          <ion-col
            class="text-container"
            size-lg="7"
          >
            <ion-text
              class="main-text"
              color="primary"
            >Sheet music. Free. Forever.</ion-text>
            <ion-text>
              With LibreScore, you can get all the sheet music you need – <b>for free</b>.
              <br>
              In contrast to other music sharing platforms, we don't charge you any money for getting notes. We are compatible with <a href="https://musescore.org/">MuseScore</a>, which is a free music writing software.
              <br>
              To top it all off, all of what you see right here is open source! If you feel like contributing, check out <a :href="githubUrl">this</a> page.
            </ion-text>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-toolbar>

    <div id="home-contents">
      <ion-toolbar>
        <ion-title
          size="large"
          style="font-size: 24px;"
        >Newest sheets</ion-title>

        <router-link
          to="/upload"
          slot="end"
        >
          <ion-text color="primary">upload ></ion-text>
        </router-link>
      </ion-toolbar>

      <ion-grid>
        <ion-row>
          <ion-col
            v-for="s of sheets"
            :key="s._id"
            size-md="2"
          >
            <router-link :to="'/score/'+s.scorepack">
              <img
                :src="'https://ipfs.io/ipfs/'+s.thumbnail"
                style="border: 2px solid #797D81"
              >
              <ion-title
                color="dark"
                class="ion-text-center"
              >{{ s.title }}</ion-title>
            </router-link>
          </ion-col>
        </ion-row>
      </ion-grid>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { IndexingInfo } from '@/core/indexing'
import { COLLECTION } from '@/core/indexing/db'

export default defineComponent({
  data () {
    return {
      githubUrl: 'https://github.com/LibreScore/LibreScore',
      sheets: [] as IndexingInfo[],
    }
  },
  async created () {
    const collection = await COLLECTION
    for await (const result of collection.find()) {
      this.sheets.push(result.value)
    }
  },
})
</script>

<style scoped>
  #home-header {
    margin: 4rem 1rem;
  }

  .librescore-logo {
    background: url("/img/logo-no-margin.svg");
    background-repeat: no-repeat;
    background-position: right;
    background-origin: content-box;
    right: 2em;
    padding-bottom: 10px;
    padding-top: 16px;
  }

  .text-container {
    font-size: 14px;
    letter-spacing: 0.02em;
  }

  .main-text {
    display: block;
    font-size: 44px;
    font-weight: 300;
    margin-bottom: 10px;
  }

  #home-contents {
    margin: 3rem;
  }
</style>
