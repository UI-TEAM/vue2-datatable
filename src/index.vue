<template>
  <div name="Datatable">
    <!-- <div v-if="$slots.default || HeaderSettings" class="clearfix" style="margin-bottom: 10px">
      <header-settings v-if="HeaderSettings" class="pull-right"
        :columns="columns" :support-backup="supportBackup">
      </header-settings>
      <slot />
    </div> -->

    <tbl v-bind="$props" />

    <div v-if="Pagination" class="row tab-custom m-0">
      <!-- <div class="col-sm-6" style="white-space: nowrap">
        <strong>
          {{ $i18nForDatatable('Total') }} {{ total }} {{ $i18nForDatatable(',') }}
        </strong>

      </div> -->
      <div class="col-sm-12 col-md-12 col-lg-12 col-xl-12 p-0">
        <page-size-select :query="query" :page-size-options="pageSizeOptions" class="float-right"/>
        <pagination class="pull-right" :total="total" :query="query" />
      </div>
    </div>
  </div>
</template>
<script>
import HeaderSettings from './HeaderSettings/index.vue'
import Tbl from './Table/index.vue'
import Pagination from './Pagination.vue'
import PageSizeSelect from './PageSizeSelect.vue'
import props from './_mixins/props'

export default {
  name: 'Datatable',
  mixins: [props],
  components: { HeaderSettings, Tbl, Pagination, PageSizeSelect },
  created () {
    // init query (make all the properties observable by using `$set`)
    const q = { limit: 10, offset: 0, sort: '', order: '', ...this.query }
    Object.keys(q).forEach(key => { this.$set(this.query, key, q[key]) })
  },
  watch: {
    data: {
      handler (data) {
        const { supportNested } = this
        // support nested components feature with extra magic
        if (supportNested) {
          const MAGIC_FIELD = '__nested__'
          data.forEach(item => {
            if (!item[MAGIC_FIELD]) {
              this.$set(item, MAGIC_FIELD, {
                comp: undefined, // current nested component
                visible: false,
                $toggle (comp, visible) {
                  switch (arguments.length) {
                    case 0:
                      this.visible = !this.visible
                      break
                    case 1:
                      switch (typeof comp) {
                        case 'boolean':
                          this.visible = comp
                          break
                        case 'string':
                        case 'object':
                          this.comp = comp
                          this.visible = !this.visible
                          break
                      }
                      break
                    case 2:
                      this.comp = comp
                      this.visible = visible
                      break
                  }
                }
              })
              if (supportNested === 'accordion') {
                this.$watch(
                  () => item[MAGIC_FIELD],
                  nested => {
                    // only one nested component can be expanded at the same time
                    if (data.filter(item => item[MAGIC_FIELD].visible).length < 2) return

                    data.forEach(item => {
                      if (item[MAGIC_FIELD].visible && item[MAGIC_FIELD] !== nested) {
                        item[MAGIC_FIELD].visible = false
                      }
                    })
                  },
                  { deep: true }
                )
              }
              Object.defineProperty(item, MAGIC_FIELD, { enumerable: false })
            }
          })
        }
      },
      immediate: true
    }
  }
}
</script>
<style>
/* transition effect: fade */
.fade-enter-active, .fade-leave-active {
  transition: opacity .2s;
}
.fade-enter, .fade-leave-active {
  opacity: 0;
}
.tab-custom{
  padding: 20px 0;
    /*border: 1px solid #eee;*/
    margin: 0;
    border-top: 0;
    border: 0 !important;
}
.table-bordered {
    border:0 !important;
}
</style>
