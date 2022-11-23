<template>
  <el-form label-position="top" class="form">
    <!-- <el-form-item label="Учет электроэнергии">
      <el-select v-model="form.typeCalculate">
        <el-option label="Один" :value="1">Один</el-option>
        <el-option label="Два" :value="2">Два</el-option>
      </el-select>
    </el-form-item> -->
    <el-form-item label="Стоимость электроэнергии">
      <div class="form__cost-wrapper">
        <el-input v-model="form.cost"></el-input>
        <span class="form__cost-label">руб./кВт⋅ч</span>
      </div>
    </el-form-item>
    <el-form-item label="Мощность прибора">
      <div class="form__power-wrapper">
        <el-input v-model="form.power"></el-input>
        <el-select v-model="form.notation"
          ><el-option label="Вт" value="vt">Вт</el-option
          ><el-option label="кВт" value="kvt">кВт</el-option></el-select
        >
      </div>
    </el-form-item>
    <el-form-item label="Время работы прибора в сутки">
      <div class="form__duration-fields-wrapper">
        <el-input
          placeholder="Часов"
          v-model="form.hours"
          @input="() => (form.hours > 24 ? (form.hours = 24) : '')"
        ></el-input
        ><el-input
          v-model="form.minutes"
          placeholder="Минут"
          @input="() => (form.minutes > 60 ? (form.minutes = 60) : '')"
        ></el-input>
      </div>
    </el-form-item>
    <div class="form__actions-wrapper">
      <el-button type="danger" @click="resetForm" :disabled="formFieldsState === 'edited'"
        >Сбросить</el-button
      >
      <el-button
        type="primary"
        :disabled="formFieldsState === 'edited'"
        @click="calculateAction"
        >Расчитать</el-button
      >
    </div>
  </el-form>
</template>

<script setup>
import { ref, computed } from "vue";
import { ElForm, ElInput, ElFormItem, ElSelect, ElOption, ElButton } from "element-plus";

const INIT_FORM = {
  typeCalculate: 1,
  notation: "vt",
  cost: null,
  minutes: null,
  hours: null,
  power: null,
};

const emit = defineEmits(["calculate-value"]);

const form = ref(null);

const resetForm = () => {
  form.value = { ...INIT_FORM };
};

resetForm();

const formFieldsState = computed(() =>
  JSON.stringify(form.value) === JSON.stringify(INIT_FORM) ? "edited" : "unedited"
);

const calculateAction = () => {
  const preparedHours = form.value.hours,
    preparedPower =
      form.value.notation === "vt" ? form.value.power / 1000 : form.value.power;

  const cost = preparedPower * preparedHours * form.value.cost;
  const testHours = Math.abs(preparedHours - 24);
  console.log(preparedHours, testHours);
  const consumption = preparedPower * testHours;
  emit("calculate-value", {
    cost,
    consumption,
  });
  resetForm();
};
</script>

<style lang="scss" scoped>
.form {
  &__duration-fields-wrapper {
    display: grid;
    grid-template-columns: auto auto;
    width: 100%;
    gap: 20px;
  }

  &__actions-wrapper {
    display: flex;
    justify-content: space-between;
  }

  & ::v-deep(.el-select) {
    width: 100%;
  }

  &__cost-wrapper {
    display: flex;
    gap: 10px;
    width: 100%;
  }

  &__cost-label {
    color: #888;
    min-width: 60px;
    font-size: 11px;
  }

  &__power-wrapper {
    display: grid;
    grid-template-columns: 2fr 0.5fr;
    gap: 10px;
  }
}
</style>
