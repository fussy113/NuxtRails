<template>
  <vs-row class="wrap" vs-type="flex" vs-justify="center">
    <vs-col class="colx" :key="index" type="flex" vs-justify="space-around" vs-align="center" vs-lg="3" vs-sm="5" vs-xs="12" v-for="(card, index) in 7" >
      <vs-card class="cardx">
        <div slot="header">
          <h3>testPage</h3>
        </div>
        <div slot="media">
          <p>vuesaxのてすと</p>
        </div>
        <div>
          <span>nekura頑張るぞい</span>
        </div>
        <div slot="footer">
          <vs-row vs-justify="flex-end">
            <vs-button type="gradient" color="danger" icon="favorite" @click="fav"></vs-button>
            <vs-button color="primary" icon="turned_in_not" @click="bookmark"></vs-button>
          </vs-row>
        </div>
      </vs-card>
    </vs-col>
  </vs-row>
</template>

<script>
import 'material-icons/iconfont/material-icons.css'
export default {
  methods: {
    fav: function () {
      this.$vs.notify({
        title: 'ふぁぼった！!',
        text: 'ふぁぼ',
        color: 'rgb(231, 60, 89)'
      })
    },
    bookmark: function () {
      this.$vs.notify({
        title: 'ぶくま',
        text: 'ok',
        color: 'rgb(31, 116, 255)'
      })
    }
  }
}
</script>
<style>
  .wrap {
    height: auto;
    min-height: 100%;
    padding: 0 16px;
  }
  .colx {
    margin: 0 8px;
  }
</style>
