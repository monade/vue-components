<template>
  <div class="card-dock w-100 d-flex flex-column flex-xl-row">
    <div class="col">
      <slot name="leftCol">
      </slot>
    </div>
    <div class="col" v-if="hasRightCol()">
      <slot name="rightCol">
      </slot>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';

@Component
export default class VCardDock extends Vue {
  hasRightCol() {
    return !!this.$slots.rightCol || !!this.$scopedSlots.rightCol;
  }
}
</script>
