<template>
  <div>
  <v-row v-if="items.length > 1 || show_if_single_entry">
    <v-select
      :items="items"
      v-model="selected"
      @change="$emit('update:selected', $event)"
      :label="label ? label : 'Layer'"
      :hint="hint ? hint : 'Select layer.'"
      :rules="rules ? rules : []"
      :multiple="multiselect"
      :chips="multiselect"
      item-text="label"
      item-value="label"
      persistent-hint
    >
    <template slot="selection" slot-scope="data">
      <div class="single-line" style="width: 100%">
        <v-chip v-if="multiselect" style="width: calc(100% - 20px)">
          <span>
            <v-icon style='margin-left: -10px; margin-right: 2px'>{{ data.item.icon }}</v-icon>
            {{ data.item.label }}
          </span>
        </v-chip>
        <span v-else>
          <v-icon style='margin-right: 2px'>{{ data.item.icon }}</v-icon>
          {{ data.item.label }}
        </span>
      </div>
    </template>
    <template v-slot:prepend-item v-if="multiselect">
      <v-list-item
        ripple
        @mousedown.prevent
        @click="() => {if (selected.length < items.length) { $emit('update:selected', items.map((item) => item.label))} else {$emit('update:selected', [])}}"
      >
        <v-list-item-action>
          <v-icon>
            {{ selected.length == items.length ? 'mdi-close-box' : selected.length ? 'mdi-minus-box' : 'mdi-checkbox-blank-outline' }}
          </v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>
            {{ selected.length < items.length ? "Select All" : "Clear All" }}
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-divider class="mt-2"></v-divider>
    </template>
    <template slot="item" slot-scope="data">
      <div class="single-line">
        <span>
          <v-icon style='margin-left: -2px; margin-right: 2px'>{{ data.item.icon }}</v-icon>
          {{ data.item.label }}
        </span>
      </div>
    </template>
   </v-select>
  </v-row>
 </div>
</template>

<script>
module.exports = {
  props: ['items', 'selected', 'label', 'hint', 'rules', 'show_if_single_entry', 'multiselect']
};
</script>

<style scoped>
  .v-select__selections {
    flex-wrap: nowrap !important;
    display: block !important;
    margin-bottom: -32px;
  }
  .single-line {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
  }
</style>
