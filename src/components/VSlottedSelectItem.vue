<template>
  <div class="medium-selector-item w-100 d-flex justify-content-between">
    <div class="d-flex align-items-center">
      <v-icon size="lg">{{item.icon}}</v-icon>
      <span class="ml-2">{{item.text}}</span>
    </div>
    <div class="d-flex align-items-center">
      <span class="text-muted">{{item.additional}}</span>
      <v-icon size="xs" class="ml-2" v-if="hasChevron">chevron-down</v-icon>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { SlottedSelectItem } from './VSlottedSelect.vue';
import VIcon from './VIcon.vue';

@Component({ components: { VIcon } })
export default class VSlottedSelectItem extends Vue {
  @Prop({ required: true }) readonly item!: SlottedSelectItem;
  @Prop({ default: false }) readonly hasChevron!: boolean;
}
</script>

<style lang="scss" scoped>
</style>
