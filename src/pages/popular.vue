<template>
  <v-container>
    <v-row>
      <PageInfoBox
        :subscribedText="$t('popular.header')"
        :unsubscribedText="$t('popular.header')"
      />
    </v-row>
    <v-row>
      <v-col cols="12">
        <PopularServers />
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <PopularMaps />
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <PopularMapNames />
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import Vue from "vue"
import PageInfoBox from "@/components/Layout/PageInfoBox.vue"
import PopularServers from "@/components/Popular/Servers/List.vue"
import PopularMaps from "@/components/Popular/Maps/List.vue"
import PopularMapNames from "@/components/Popular/MapNames/List.vue"
import {
  FETCH_POPULAR,
  FETCH_SERVERS,
  FETCH_LEVEL_NAMES,
} from "@/store/actions.type"

export default Vue.extend({
  middleware: ["auth"],
  layout: "app",
  name: "Popular",
  components: {
    PageInfoBox,
    PopularServers,
    PopularMaps,
    PopularMapNames,
  },
  transition: {
    css: true,
  },
  mounted() {
    this.fetchPopular()
  },
  methods: {
    fetchPopular() {
      this.$store.dispatch(FETCH_POPULAR).catch((err: Error) => {
        this.$toast.error(err.message)
      })
      this.$store.dispatch(FETCH_SERVERS).catch((err: Error) => {
        this.$toast.error(err.message)
      })
      this.$store.dispatch(FETCH_LEVEL_NAMES).catch((err: Error) => {
        this.$toast.error(err.message)
      })
    },
  },
  head() {
    return {
      title: String(this.$t("popular.pageTitle")),
    }
  },
})
</script>
