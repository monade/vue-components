<template>
  <div class="v-dockable-card shadow-sm w-100">
    <div class="v-dockable-card-header" v-if="showHeader">
      <slot name="header">
        <h6>{{title}}</h6>
      </slot>
    </div>
    <div class="v-dockable-card-container">
      <slot name="body">
        <p>Body</p>
      </slot>
    </div>
    <div class="v-dockable-card-footer" v-if="hasFooter">
      <slot name="footer">
      </slot>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';

@Component
export default class VDockableCard extends Vue {
  @Prop({ default: () => 'Titolo' }) readonly title!: string;
  @Prop({ default: true }) readonly showHeader!: boolean;

  get hasFooter() {
    return !!this.$slots.footer
  }
}

</script>

<style lang="scss" scoped>
@import '@/css/vue';
.v-dockable-card {
  background-color: $white;
  border: 1px solid $gray-300;
  &-header {
    padding: 1rem 0.5rem;
    border-bottom: 1px solid $gray-300;
    h6 {
      margin: 0;
    }
  }
  &-container {
    padding: 1rem 0.5rem;
  }
  &-footer {
    border-top: 1px solid $gray-300;
    padding: 1rem 0.5rem;
  }
}
</style>
