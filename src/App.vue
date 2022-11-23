<template>
  <el-card class="box-card">
    <template #header>
      <div class="box-card__card-header">
        <span>Калькулятор расчета электроэнергии</span>
      </div>
    </template>
    <calculating-form @calculate-value="calculateValue" />
    <div v-if="results">
      <p>Стоимость</p>
      <div>За сутки {{ results.cost }}Р</div>
      <div>За месяц {{ results.cost * 30 }}Р</div>
      <div>За год {{ results.cost * 365 }}Р</div>
      <p>Расход электроэнергии</p>
      <div>За сутки {{ results.consumption }}кВт</div>
      <div>За месяц {{ results.consumption }}кВт</div>
      <div>За год {{ results.consumption }}кВт</div>
    </div>
  </el-card>
</template>

<script setup>
import { ref } from "vue";
import { ElCard } from "element-plus";
import CalculatingForm from "./components/CalculatingForm.vue";

const results = ref(null);

const calculateValue = (value) => {
  results.value = { ...value };
};
</script>

<style lang="scss" scoped>
.box-card {
  &__card-header {
    display: flex;
    gap: 10px;
    align-items: center;
    justify-content: space-between;
  }
}
</style>
