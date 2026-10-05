<template>
  <div class="settings-page">
    <div class="container page">
      <div class="row">
        <div class="col-md-6 offset-md-3 col-xs-12">
          <h1 class="text-xs-center">Your Settings</h1>
          <RwvListErrors v-if="hasErrors" :errors="errors" />
          <form @submit.prevent="updateSettings()">
            <fieldset :disabled="inProgress">
              <fieldset class="form-group">
                <input
                  class="form-control"
                  type="text"
                  name="image"
                  v-model="form.image"
                  placeholder="URL of profile picture"
                />
              </fieldset>
              <fieldset class="form-group">
                <input
                  class="form-control form-control-lg"
                  type="text"
                  name="username"
                  v-model="form.username"
                  placeholder="Your username"
                />
              </fieldset>
              <fieldset class="form-group">
                <textarea
                  class="form-control form-control-lg"
                  rows="8"
                  name="bio"
                  v-model="form.bio"
                  placeholder="Short bio about you"
                ></textarea>
              </fieldset>
              <fieldset class="form-group">
                <input
                  class="form-control form-control-lg"
                  type="text"
                  name="email"
                  v-model="form.email"
                  placeholder="Email"
                />
              </fieldset>
              <fieldset class="form-group">
                <input
                  class="form-control form-control-lg"
                  type="password"
                  name="password"
                  v-model="form.password"
                  placeholder="Password"
                />
              </fieldset>
              <button
                :disabled="inProgress"
                type="submit"
                class="btn btn-lg btn-primary pull-xs-right"
              >
                {{ inProgress ? "Updating..." : "Update Settings" }}
              </button>
            </fieldset>
          </form>
          <hr />
          <button @click="logout" class="btn btn-outline-danger">
            Or click here to logout.
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, onMounted, reactive, ref, watch } from "vue";
import { storeToRefs } from "pinia";
import { useRouter } from "vue-router";
import { useAuthStore } from "@/store/auth";
import RwvListErrors from "@/components/ListErrors";
import { extractErrors } from "@/common/errors";

defineOptions({ name: "RwvSettings" });

const router = useRouter();
const authStore = useAuthStore();
const { currentUser } = storeToRefs(authStore);

const errors = ref({});
const inProgress = ref(false);
const form = reactive({
  username: "",
  email: "",
  bio: "",
  image: "",
  password: ""
});

const hasErrors = computed(() => Object.keys(errors.value).length > 0);

const syncFromUser = () => {
  const u = currentUser.value || {};
  form.username = u.username || "";
  form.email = u.email || "";
  form.bio = u.bio || "";
  form.image = u.image || "";
};

onMounted(syncFromUser);
watch(() => currentUser.value.username, syncFromUser);

const updateSettings = () => {
  inProgress.value = true;
  const payload = {
    username: form.username,
    email: form.email,
    bio: form.bio,
    image: form.image
  };
  if (form.password) payload.password = form.password;
  authStore
    .updateUser(payload)
    .then(() => {
      inProgress.value = false;
      errors.value = {};
      router.push({
        name: "profile",
        params: { username: form.username }
      });
    })
    .catch((error) => {
      inProgress.value = false;
      errors.value = extractErrors(error);
    });
};

const logout = () => {
  authStore.logout();
  router.push({ name: "home" });
};
</script>
