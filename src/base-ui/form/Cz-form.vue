<template>
  <div class="cz-form-search">
    <el-form :label-width="labelWidth" label-position="left">
      <el-row>
        <template v-for="item in formItems" :key="item.label">
          <el-col v-bind="colLayout">
            <el-form-item :label="item.label" v-if="!item.isHidden">
              <template v-if="item.type === 'input'">
                <el-input v-model="formData[item.field]"></el-input>
              </template>
              <template
                v-else-if="item.type === 'password'"
                :label="item.label"
              >
                <el-input
                  show-password
                  v-model="formData[item.field]"
                ></el-input>
              </template>
              <template v-else-if="item.type === 'select'" :label="item.label">
                <el-select
                  v-model="formData[item.field]"
                  class="m-2"
                  placeholder="Select"
                  size="large"
                  style="width: 100%"
                >
                  <el-option
                    v-for="option in item.selectOptions"
                    :key="option.value"
                    :value="option.value"
                    :label="option.label"
                  >
                  </el-option>
                </el-select>
              </template>
              <template v-else :label="item.label">
                <el-date-picker
                  v-model="formData[item.field]"
                  type="daterange"
                  range-separator="To"
                  start-placeholder="Start date"
                  end-placeholder="End date"
                  style="width: 100%"
                >
                </el-date-picker>
              </template>
            </el-form-item>
          </el-col>
        </template>
      </el-row>
    </el-form>
    <div class="footer">
      <slot name="btns"></slot>
    </div>
  </div>
</template>

<script lang="ts">
import { PropType, defineComponent, ref, watch } from "vue"
import { FormItemOptions } from "@/base-ui/form/types"
export default defineComponent({
  props: {
    labelWidth: {
      type: String,
      default: "120px",
    },
    formItems: {
      type: Array as PropType<FormItemOptions[]>,
      default: () => [],
    },
    colLayout: {
      type: Object,
      default: {
        xl: 6,
        lg: 8,
        md: 12,
        sm: 24,
        xs: 24,
      },
    },
    modelValue: {
      type: Object,
      default: () => ({}),
    },
  },
  emits: ["update:modelValue"],
  setup(props, { emit }) {
    const formData = ref({ ...props.modelValue })
    watch(
      formData,
      newVal => {
        emit("update:modelValue", newVal)
      },
      { deep: true }
    )
    return {
      formData,
    }
  },
})
</script>

<style scoped lang="less">
.header {
  height: 100px;
}
.el-row {
  // padding: 10px;
  .el-col {
    padding: 10px 30px;
  }
}
</style>
