<template>
  <div class="arguments-wrapper">
    <b-field :label="$t(label)" class="balance">
      <b-input v-model="inputValue" @input="handleInput" type="number" step="0.001" min="0"/>
      <p class="control balance">
        <b-select :disabled="!calculate" v-model="selectedUnit" @input="handleInput">
          <option v-for="u in units" v-bind:key="u.value" v-bind:value="u.value">
            {{ u.name }}
          </option>
        </b-select>
      </p>
    </b-field>
  </div>
</template>

<script lang="ts" >
import { Component, Prop, Emit, Mixins } from 'vue-property-decorator'
import Balance from '@/params/components/Balance.vue'
import { units as defaultUnits } from '@/params/constants'
import { Unit } from '@/params/types'
import { Debounce } from 'vue-debounce-decorator'
import ChainMixin from '@/utils/mixins/chainMixin'

const components = { Balance }

@Component({ components })
export default class BalanceInput extends Mixins(ChainMixin) {
  @Prop({ type: [Number, String], default: 0 }) value!: number;
  protected units: Unit[] = defaultUnits;
  private selectedUnit = 1;
  @Prop({ default: 'balance' }) public label!: string;
  @Prop({ default: true }) public calculate!: boolean;

  get inputValue(): number {
    return this.value
  }

  set inputValue(value: number) {
    this.handleInput(value)
  }

  formatSelectedValue(value: number): number {
    return  value * (10**this.decimals) * this.selectedUnit
  }

  get calculatedBalance() {
    return this.formatSelectedValue(this.inputValue)
  }

  protected mapper(unit: Unit) {
    if (unit.name === '-') {
      return { ...unit, name: this.unit }
    }
    return unit
  }

  public mounted() {
    this.units = defaultUnits.map(this.mapper)
  }

  @Debounce(200)
  @Emit('input')
  public handleInput(value: number) {
    return this.calculate ? this.formatSelectedValue(value) : value
  }
}
</script>
