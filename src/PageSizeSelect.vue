<template>
  <label name="PageSizeSelect" class="ml-10">
    <select class="form-control input-sm -page-size-select fs-12 f-clr" v-model="query.limit"
      @change="query.offset = 0 /* reset to the first page */">
      <option v-for="i in pageSizeOptions" :value="i">{{ i }}/page</option>
    </select>
    <!-- {{ $i18nForDatatable('items / page') }} -->
  </label>
</template>
<script>
export default {
  name: 'PageSizeSelect',
  props: {
    query: { type: Object, required: true },
    pageSizeOptions: { type: Array, required: true }
  }
}
</script>
<style>
label{
  margin-bottom: 0;
}
.-page-size-select {
    display: inline-block;
    width: 80px;
    padding: 5px 5px !important;
    font-size: 12px !important;
}
select.form-control:not([size]):not([multiple]) {
    height: calc(2.25rem + -4px) !important;
}
</style>
