<template>
  <div class="form-box">
    <div class="card">
      <el-form :model="formData" label-width="140px">
        <el-form-item>
          <el-button type="primary" :icon="Upload" @click="batchAdd"> 批量添加数据 </el-button>
          <ImportExcel ref="importRef" />
        </el-form-item>
        <el-form-item label="特征起始位置:">
          <el-input v-model="formData.index1" placeholder="请输入特征起始位置" clearable />
        </el-form-item>
        <el-form-item label="特征终止位置:">
          <el-input v-model="formData.index2" placeholder="请输入特征终止位置" clearable />
        </el-form-item>
        <el-form-item label="选择数据类型结构:">
          <el-select v-model="formData.datatype" placeholder="请选择数据集类型">
            <el-option label="结构化数据" value="1" />
            <el-option label="时序数据" value="2" />
          </el-select>
        </el-form-item>
        <el-form-item label="选择模型:">
          <el-select v-model="formData.datatype" placeholder="请选模型">
            <el-option label="MEFDPN" value="1" />
            <el-option label="GAO-VMD-SE" value="2" />
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit"> Create </el-button>
          <el-button>Cancel</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="card">
      <el-form ref="formRef" :model="dynamicValidateForm" label-width="100px" class="demo-dynamic">
        <el-form-item>
          <el-button class="add" type="primary" plain @click="addDomain"> Add Input </el-button>
        </el-form-item>
        <el-form-item
          prop="param"
          label="模型"
          :rules="[
            {
              required: true,
              message: '请输入模型参数',
              trigger: 'blur'
            },
            {
              type: 'email',
              message: '请输入模型参数',
              trigger: ['blur', 'change']
            }
          ]"
        >
          <el-input v-model="dynamicValidateForm.email" />
        </el-form-item>
        <el-form-item
          v-for="(domain, index) in dynamicValidateForm.domains"
          :key="domain.key"
          :label="'参数' + index"
          :prop="'params.' + index + '.value'"
          :rules="{
            required: true,
            message: 'domain can not be null',
            trigger: 'blur'
          }"
        >
          <el-input v-model="domain.value">
            <template #append>
              <el-button type="danger" plain class="mt-2" @click.prevent="removeDomain(domain)"> Delete </el-button>
            </template>
          </el-input>
        </el-form-item>
        <!-- <el-form-item>
          <el-button type="primary" @click="submitForm(formRef)"> Submit </el-button>
          <el-button @click="resetForm(formRef)"> Reset </el-button>
        </el-form-item> -->
      </el-form>
    </div>
  </div>
</template>

<script setup lang="ts" name="proForm">
import { ref, reactive } from "vue";
import { Upload } from "@element-plus/icons-vue";
import { exportUserInfo, BatchAddUser } from "@/api/modules/user";
import { ElMessage } from "element-plus";
import ImportExcel from "@/components/ImportExcel/index.vue";

// do not use same name with ref
const formData = reactive({
  index1: "",
  index2: "",
  datatype: ""
});
const importRef = ref();

const batchAdd = () => {
  let params = {
    title: "数据",
    tempApi: exportUserInfo,
    importApi: BatchAddUser
  };
  importRef.value.acceptParams(params);
};

const onSubmit = () => {
  ElMessage.success("提交的数据为 : " + JSON.stringify(formData));
};

interface DomainItem {
  key: number;
  value: string;
}
const dynamicValidateForm = reactive<{
  domains: DomainItem[];
  email: string;
}>({
  domains: [
    {
      key: 1,
      value: ""
    }
  ],
  email: ""
});

const removeDomain = (item: DomainItem) => {
  const index = dynamicValidateForm.domains.indexOf(item);
  if (index !== -1) {
    dynamicValidateForm.domains.splice(index, 1);
  }
};

const addDomain = () => {
  dynamicValidateForm.domains.push({
    key: Date.now(),
    value: ""
  });
};
</script>

<style scoped lang="scss">
@import "./index.scss";
</style>
