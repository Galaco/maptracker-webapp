<template>
  <v-card>
    <v-toolbar flat>
      <v-toolbar-title>
        {{ $t("popular.mapNames.title") }}
      </v-toolbar-title>
    </v-toolbar>
    <v-card-text>
      <v-simple-table v-if="isPopularItemsLoading === false">
        <thead v-if="popularItems.watchedMaps.length > 0">
          <tr>
            <th>{{ $t("servers.list.headers.server") }}</th>
            <th class="controls">
              {{ $t("table.headers.actions") }}
            </th>
          </tr>
        </thead>
        <tbody
          v-if="popularItems.watchedMaps.length > 0 && levelNamesCount > -1"
        >
          <Item
            v-for="(map, index) in popularItems.watchedMaps"
            :key="index"
            :model="map"
            :can-user-add="!levelNames.find(s => s.name === map.name)"
          />
        </tbody>
      </v-simple-table>
      <div v-if="isPopularItemsLoading === true" class="row text-center">
        <div class="col-sm-12 loading-spinner">
          <Spinner />
        </div>
      </div>
      <div
        v-if="
          isPopularItemsLoading === false &&
          popularItems.watchedMaps.length == 0
        "
        class="row text-center"
      >
        <div class="col-sm-12 loading-spinner">
          {{ $t("popular.mapNames.insufficientData") }}
        </div>
      </div>
    </v-card-text>
  </v-card>
</template>

<script lang="ts">
import Vue from "vue"
import { mapGetters } from "vuex"
import Item from "./Item.vue"
import Spinner from "@/components/LoadingIndicator/Spinner.vue"

export default Vue.extend({
  name: "PopularMapNames",
  components: {
    Item,
    Spinner,
  },
  computed: {
    ...mapGetters([
      "popularItems",
      "isPopularItemsLoading",
      "levelNames",
      "levelNamesCount",
    ]),
  },
})
</script>

<style lang="scss" scoped>
.loading-spinner {
  height: 240px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
