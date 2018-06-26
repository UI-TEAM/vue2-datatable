<template>
  <tbody>
    <template v-if="data.length">
      <template v-for="item in data">
        <tr>
          <td v-if="shouldRenderSelection">
            <multi-select :selection="selection" :row="item" />
          </td>
          <td v-for="col in columns" :class="col.tdClass" :style="col.tdStyle">
            <!-- <td> component (tdComp) -->
            <component
              v-if="col.tdComp"
              :is="forDynCompIs(col.tdComp)"
              :row="item"
              :field="col.field"
              :value="item[col.field]"
              :nested="item.__nested__"
              v-bind="$props">
            </component>
            <template v-else>
              {{ item[col.field] }}
            </template>
          </td>
        </tr>
        <transition name="fade">
          <tr v-if="item.__nested__ && item.__nested__.visible">
            <td :colspan="colLen">
              <!-- nested component -->
              <component
                :is="forDynCompIs(item.__nested__.comp)"
                :row="item"
                :nested="item.__nested__"
                v-bind="$props">
              </component>
            </td>
          </tr>
        </transition>
      </template>
    </template>
    <tr v-else-if="!leftFixed && !rightFixed">
      <td :colspan="colLen" class="text-center text-muted">
        ( {{ $i18nForDatatable('No Data') }} )
      </td>
    </tr>
  </tbody>
</template>
<script>
import MultiSelect from './MultiSelect.vue'
import props from '../_mixins/props'
import shouldRenderSelection from '../_mixins/shouldRenderSelection'

export default {
  name: 'TableBody',
  components: { MultiSelect },
  mixins: [props, shouldRenderSelection],
  computed: {
    colLen () {
      return this.columns.length + !!this.selection
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
tbody{
  color:#616161 !important;
  border: 0 !important;
}
tbody tr{
  border-bottom: 1px solid #eee;
}
tbody tr td:first-child{
min-width: 42px !important;
}
.table-striped tbody tr:nth-of-type(odd) {
background-color: #fff;
}
.table-striped tbody tr:nth-of-type(even):hover, .table-striped tbody tr:nth-of-type(odd):hover {
/*background-color: #f5f5f5;*/
background-color: rgba(128, 128, 128, 0.04) !important;
}
.table-bordered th, .table-bordered td {
    border: 0;
}
.table th, .table td {
    /*padding: 1rem;*/
    word-wrap: break-word;
}
</style>
