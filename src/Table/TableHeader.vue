<template>
  <thead>
    <transition-group name="fade" tag="tr">
      <th v-if="shouldRenderSelection" key="--th-multi">
        <multi-select :selection="selection" :rows="data" />
      </th>
      <th v-for="(col, idx) in columns"
        :key="col.title || col.field || idx"
        :class="col.thClass" :style="col.thStyle">
        <!-- <th> component (thComp) -->
        <component
          v-if="col.thComp"
          :is="forDynCompIs(col.thComp)"
          :column="col"
          :field="col.field"
          :title="col.title"
          v-bind="$props">
        </component>
        <template v-else>
          {{ col.title }}
        </template>

        <i v-if="col.explain" class="fa fa-info-circle" style="cursor: help" :title="col.explain"></i>
        <head-sort v-if="col.sortable" :field="col.field" :query="query" />
      </th>
    </transition-group>
  </thead>
</template>
<script>
import HeadSort from './HeadSort.vue'
import MultiSelect from './MultiSelect.vue'
import props from '../_mixins/props'
import shouldRenderSelection from '../_mixins/shouldRenderSelection'

export default {
  name: 'TableHeader',
  components: { HeadSort, MultiSelect },
  mixins: [props, shouldRenderSelection]
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
thead{
  color:#616161 !important;
  border-bottom: 1px solid #eee !important;
}
.table-bordered {
    border:0 !important;
}
.table-bordered thead th:first-child{
min-width: 42px !important;
}
.table-bordered thead th, .table-bordered thead td {
    border:0 !important;
    font-weight: normal;
}
.table th, .table td {
    /*padding: 1rem;*/
    word-wrap: break-word;
}
</style>
