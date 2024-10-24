<template>
  <v-container class="fill-height">
    <v-responsive class="align-centerfill-height mx-auto" max-width="900">
      <v-row justify="center">
        <v-col key="1" cols="12" md="6">
          <v-card color="primary" variant="outlined" class="mx-auto">
            <v-card-text>
              <div>
                <div class="text-overline mb-1">Sign-in Email</div>
                <div class="text-h6 mb-1">{{ signInSubject }}</div>
                <br />
                <div class="text-medium-emphasis">
                  {{ signInBody }}
                </div>
                <br />
                <div class="text-medium-emphasis">
                  {{ signInSignature }}
                </div>
              </div>
            </v-card-text>

            <v-card-actions>
              <v-btn @click="generateSignInEmail">
                Regenerate Sign-in Email
              </v-btn>
              <v-btn @click="openOutlookEmail" :disabled="siSendBtnDisabled">
                Send Email
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
      <v-row justify="center">
        <v-col key="1" cols="12" md="6">
          <v-card color="primary" variant="outlined" class="mx-auto">
            <v-card-text>
              <div>
                <div class="text-overline mb-1">Sign-out Email</div>
                <div class="text-h6 mb-1">{{ signOutSubject }}</div>
                <br />
                <div class="text-medium-emphasis mb-1">
                  {{ signOutBody }}
                </div>
                <br />
                <div class="text-medium-emphasis mb-1">
                  {{ signOutSignature }}
                </div>
              </div>
            </v-card-text>

            <v-card-actions>
              <v-btn @click="generateSignOutEmail">
                Regenerate Sign-out Email
              </v-btn>
              <v-btn
                @click="openOutlookEmail(false)"
                :disabled="soSendBtnDisabled"
              >
                Send Email
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-responsive>
  </v-container>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import {
  checkInSubjects,
  signOutSubjects,
  checkInBodies,
  signOutBodies,
  MondaycheckInBodies,
  FridaysignOutBodies,
  signature,
} from "@/utils/mailBody";
const signInSubject = ref("");
const signInBody = ref("");
const signInSignature = ref("");
const signOutSubject = ref("");
const signOutBody = ref("");
const signOutSignature = ref("");
const soSendBtnDisabled = ref(true);
const siSendBtnDisabled = ref(true);
const mailTo = ref("");
const ccTo = ref("");

onMounted(() => {
  const localMalto = localStorage.getItem("mailTo");
  const localCC = localStorage.getItem("ccTo");
  mailTo.value = localMalto ?? "";
  ccTo.value = localCC ?? "";
});

const generateSignInEmail = () => {
  const today = new Date().getDay();

  signInSubject.value =
    checkInSubjects[Math.floor(Math.random() * checkInSubjects.length)];
  signInSignature.value =
    signature[Math.floor(Math.random() * signature.length)];
  if (today === 1)
    signInBody.value =
      MondaycheckInBodies[
        Math.floor(Math.random() * MondaycheckInBodies.length)
      ];
  else
    signInBody.value =
      checkInBodies[Math.floor(Math.random() * checkInBodies.length)];
  siSendBtnDisabled.value = false;
};

const generateSignOutEmail = () => {
  const today = new Date().getDay();

  signOutSubject.value =
    signOutSubjects[Math.floor(Math.random() * signOutSubjects.length)];
  signOutSignature.value =
    signature[Math.floor(Math.random() * signature.length)];
  if (today === 5)
    signOutBody.value =
      FridaysignOutBodies[
        Math.floor(Math.random() * FridaysignOutBodies.length)
      ];
  else
    signOutBody.value =
      signOutBodies[Math.floor(Math.random() * signOutBodies.length)];
  soSendBtnDisabled.value = false;
};

const openOutlookEmail = (signIn: boolean = true) => {
  let mailBody = `${signInBody.value}\n\n${signInSignature.value}`;
  let subject = signInSubject.value;
  if (!signIn) {
    mailBody = `${signOutBody.value}\n\n${signOutSignature.value}`;
    subject = signOutSubject.value;
  }
  const mailToLink = `mailto:${encodeURIComponent(
    mailTo.value
  )}?cc=${encodeURIComponent(ccTo.value)}&subject=${encodeURIComponent(
    subject
  )}&body=${encodeURIComponent(mailBody)}`;
  window.location.href = mailToLink;
};
</script>
