<template>
  <div class="slotted-select" v-click-outside="closeDropdown">
    <div class="form-group">
      <label>{{label}}</label>
      <div class="slotted-select-selected d-flex h-100" @click="toggle">
        <slot name="selected" :selected="selectedItem">
        </slot>
      </div>
    </div>
    <div class="w-100 slotted-select-options shadow-sm " v-if="isOpen" :style="{'height': dropDownSize+'px'}">
      <div class="slotted-select-options-grouped" v-if="hasGroups">
        <div class="slotted-select-options-grouped-block" v-for="group in groups" :key="group.name">
          <div class="slotted-select-options-grouped-block-header">
            <slot name="group-header" v-bind:group="group">
            </slot>
          </div>
          <div class="slotted-select-options-items d-flex h-100" v-for="item in group.items" :key="item.id" @click="select(item, group)">
            <slot name="item" v-bind:item="item">
            </slot>
          </div>
        </div>
      </div>
      <div class="slotted-select-options-grouped" v-else>
        <div class="slotted-select-options-items d-flex h-100" v-for="item in items" :key="item.id" @click="select(item)">
          <slot name="item" :item="item">
          </slot>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';
import ClickOutside from '../directives/ClickOutside';

export type SlottedSelectGroup = {
  id: string;
  name: string;
  items: SlottedSelectItem[];
}

export type SlottedSelectItem = {
  icon: string | null;
  id: string;
  text: string;
  additional: string | null;
}

@Component({ directives: { ClickOutside } })
export default class VSlottedSelect extends Vue {
  @Prop({ default: 'Label' }) readonly label!: string;
  @Prop({ required: true }) readonly selected!: SlottedSelectItem;
  @Prop({ default: () => { return [] } }) readonly items!: SlottedSelectItem[];
  @Prop({ default: () => { return [] } }) readonly groups!: SlottedSelectGroup[];
  @Prop({ default: 400 }) readonly dropDownSize!: number;

  private selectedItem = Object.assign({}, this.selected);
  isOpen = false;

  get hasGroups() {
    return this.groups.length > 0;
  }

  toggle() {
    this.isOpen = !this.isOpen;
  }

  closeDropdown() {
    this.isOpen = false;
  }

  select(item: SlottedSelectItem, group?: SlottedSelectGroup) {
    this.selectedItem = item;
    this.$emit('selected', item, group);
    this.isOpen = false;
  }
}
</script>
<style lang="scss" scoped>
@import "@/css/vue";
.slotted-select {
  color: $black;
  width: 100%;
  user-select: none;
  position: relative;
  .form-group {
    label {
      color: $black;
    }
    margin-bottom: 5px;
  }
  &-selected {
    background-color: $white;
    border-radius: 4px;
    padding: .5rem 1rem;
    border: 1px solid $gray-300;
    color: $black;
  }
  &-options {
    z-index: 11;
    overflow-y: auto;
    background-color: $white;
    position: absolute;
    border-radius: 4px;
    border: 1px solid $gray-300;
    &-items {
      padding: .25rem 1rem 0.25rem 1rem;
    }
    &-items:first-child {
      padding: .5rem 1rem 0.25rem 1rem;
    }
    &-items:last-child {
      padding: .25rem 1rem 0.5rem 1rem;
    }
    &-items:hover {
      background-color: $primary;
      color: $white;
    }
    &-grouped-block-header {
      padding: 0.125rem;
    }
  }
}
</style>
