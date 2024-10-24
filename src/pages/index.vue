<template>
  <v-row>
    <v-col>
      <v-form @submit.prevent="onClickSave">
        <v-container>
          <v-row>
            <v-col cols="12" md="4">
              <v-text-field
                v-model="mailTo"
                label="Mail To"
                required
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-text-field v-model="ccTo" label="CC" required></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-btn
                class="mt-2"
                type="submit"
                block
                color="success"
                :disabled="mailTo === '' && ccTo === ''"
              >
                Submit
              </v-btn>
              <v-btn
                :disabled="mailTo === '' || ccTo === ''"
                class="mt-2"
                block
                prepend-icon="mdi-trash-can-outline"
                color="error"
                @click="onClickClear"
              >
                Reset
              </v-btn>
            </v-col>
          </v-row>
        </v-container>
      </v-form>
    </v-col>
  </v-row>
  <v-row>
    <v-col>
      <HelloWorld />
    </v-col>
  </v-row>
</template>

<script lang="ts" setup>
import { onMounted, ref } from "vue";

const mailTo = ref("");
const ccTo = ref("");

onMounted(() => {
  const localMalto = localStorage.getItem("mailTo");
  const localCC = localStorage.getItem("ccTo");
  mailTo.value = localMalto ?? "";
  ccTo.value = localCC ?? "";
});

const onClickClear = () => {
  localStorage.removeItem("mailTo");
  localStorage.removeItem("ccTo");
  mailTo.value = "";
  ccTo.value = "";
};

const onClickSave = () => {
  localStorage.setItem("mailTo", mailTo.value);
  localStorage.setItem("ccTo", ccTo.value);
};
</script>
