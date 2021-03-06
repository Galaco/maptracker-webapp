<template>
  <tr>
    <td class="v-center">
      {{ model.name }}
    </td>
    <td class="v-center">
      {{ getServerNameFromId(model.serverId) }}
    </td>
    <td class="v-center">
      <v-switch v-model="isEnabled" @change="onDisable" />
    </td>
    <td class="controls" align="center">
      <v-tooltip top>
        <template v-slot:activator="{ on }">
          <v-btn
            class="mx-2"
            fab
            dark
            small
            color="pink"
            v-on="on"
            @click="onDelete(model)"
          >
            <v-icon>mdi-delete</v-icon>
          </v-btn>
        </template>
        <span>{{ $t("notifications.buttons.delete") }}</span>
      </v-tooltip>
    </td>
  </tr>
</template>

<script lang="ts">
import Vue from "vue"
import { DISABLE_MAP_NAME } from "@/store/actions.type"
import WatchedMap from "@/models/api/maps/WatchedMap"

export default Vue.extend({
  name: "MapRow",
  props: {
    model: {
      type: WatchedMap,
      default: new WatchedMap("", ""),
    },
    getServerNameFromId: {
      type: Function,
      default: () => "",
    },
    onDelete: {
      type: Function,
      default: () => {},
    },
  },
  data() {
    return {
      isEnabled: true,
    }
  },
  mounted() {
    this.isEnabled = !this.model.disabled
  },
  methods: {
    onDisable(value: boolean) {
      this.$store
        .dispatch(DISABLE_MAP_NAME, { model: this.model, isDisabled: !value })
        .then(() => {
          this.model.disabled = !value
          this.isEnabled = value
          if (value === true) {
            this.$toast.success(
              this.$t("notifications.toast.enable.success", {
                name: this.model.name,
              })
            )
          } else {
            this.$toast.success(
              this.$t("notifications.toast.disable.success", {
                name: this.model.name,
              })
            )
          }
        })
        .catch(err => {
          this.$toast.error(err.message)
        })
    },
  },
})
</script>

<style lang="scss" scoped>
th.controls {
  text-align: center;
  width: 160px;
  min-width: 160px;
}


@media only screen and (max-width: 585px) {
  td {
    height: 32px;
    
    &:first-child {
      display: block;
      border-bottom: 0 !important;
      line-height: 36px;
      font-weight: bold;
      height: 36px;
      font-size: 16px;
    }

    &:nth-child(2) {
      display: block;
      float: left;
      width: 100%;
    }

    &:last-child {
      display: none;
    }
  }
}
</style>
