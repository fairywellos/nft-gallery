<template>
  <b-field :label="$t(label)">
    <b-input :placeholder="placeholder" v-model="vValue" :expanded="expanded" />
  </b-field>
</template>

<script lang="ts" >
import { Component, Prop, VModel, Vue } from 'vue-property-decorator'

@Component
export default class BasicInput extends Vue {
  @VModel({ type: String, required: true }) vValue!: string;
  @Prop({ type: String, required: true }) label!: string;
  @Prop({ type: String, required: true }) placeholder!: string;
  @Prop({ type: Boolean, default: false }) expanded!: boolean;
}
</script>
