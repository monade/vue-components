<template>
  <v-multi-select
    :value="selected"
    @input="onChanged"
    :placeholder="disabled ? '-' : placeholder"
    :options="options"
    :disabled="disabled"
    :confirmSelection="false"
    :showDropdown="showDropdown"
    :searchEnabled="searchEnabled"
  />
</template>

<script lang="ts">
import { Component, Vue, Prop, Watch } from 'vue-property-decorator';
import VMultiSelect, { MultiSelectOption } from './VMultiSelect.vue';
import { renderNestedParams } from '../utils/renderNestedParams';

@Component({ components: { VMultiSelect } })
export default class VInlineMultiSelect extends Vue {
  @Prop({ default: () => [] }) readonly value!: any[] | null;
  @Prop({ default: 'Seleziona un valore' }) readonly placeholder!: string;
  @Prop({ default: 'name' }) readonly label!: string;
  @Prop({ default: false }) readonly disabled!: boolean;
  @Prop({ default: false }) readonly showDropdown!: boolean;
  @Prop({ default: false }) readonly searchEnabled!: boolean;
  @Prop() readonly dataKeys?: string[];
  @Prop({ required: true }) readonly options!: MultiSelectOption[];

  private selected: any[] | null = null;
  private preventUpdate = false;

  created() {
    if (!this.value) {
      return;
    }
    this.selected = this.options.filter(e => this.value?.includes(e) || this.value?.find(s => s === e?.id));
  }

  @Watch('value')
  onValueChanged(value: any[], oldVal: any[]) {
    if (!value || value === oldVal) {
      // this.preventUpdate = true;
      return;
    }

    if (value.length === this.selected?.length) {
      if (value.every((e, index) => this.selected && e === this.selected[index]?.id)) {
        // this.preventUpdate = true;
        return;
      }
    }
    this.selected = this.options.filter(e => value.includes(e) || value.find(s => s === e?.id));
  }

  onChanged(value?: MultiSelectOption[]) {
    if (!this.disabled) {
      this.$emit('change', this.outputParams(value));
    }
  }

  outputParams(value?: MultiSelectOption[]) {
    return renderNestedParams(this.dataKeys, value?.map(e => e.id));
  }
}
</script>
