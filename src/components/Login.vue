<template>
  <div>
    <q-layout>
      <q-page-container>
        <q-page class="bg-primary flex flex-center" v-if="$q.platform.is.desktop">
          <div class="full-width">
            <div class="row justify-center">
              <div class="col-xs-10 col-sm-6 col-md-5 col-lg-4">
                <q-card color="white">
                  <q-card-section class="q-px-xl q-py-md">
                    <q-img v-bind:src="logoImg" />
                  </q-card-section>

                  <q-card-section>
                    <q-input :label="$t('username')" v-model="username" />

                    <q-input :label="$t('password')" type="password" v-model="password" />
                  </q-card-section>
                  <q-card-actions class="q-pa-md" align="between">
                    <q-checkbox v-model="rememberMe" :label="$t('remember_me')" class="text-tertiary" />
                    <q-checkbox v-model="useSSL" :label="$t('use_ssl')" class="text-tertiary" />
                    <q-btn icon="check" :label="$t('login')" color="positive" @click="login" />
                  </q-card-actions>
                </q-card>
              </div>
            </div>
          </div>
        </q-page>
        <q-page
          v-else-if="$q.platform.is.mobile"
          class="bg-primary window-height window-width row justify-center items-center"
        >
          <div class="column">
            <div class="row">
              <h5 class="text-h5 text-white q-my-md">Freedomotic</h5>
            </div>
            <div class="row">
              <q-card square bordered class="q-pa-lg shadow-1">
                <q-card-section>
                  <q-form class="q-gutter-md">
                    <q-input square filled clearable v-model="username" label="username" />
                    <q-input
                      square
                      filled
                      clearable
                      v-model="password"
                      type="password"
                      label="password"
                    />
                  </q-form>
                </q-card-section>
                <q-card-actions class="q-px-md">
                  <q-btn
                    unelevated
                    color="light-green-7"
                    size="lg"
                    class="full-width"
                    label="Login"
                    @click="login"
                  />
                </q-card-actions>
                <q-card-section class="text-center q-pa-none"></q-card-section>
              </q-card>
            </div>
          </div>
        </q-page>
      </q-page-container>
    </q-layout>
  </div>
</template>



<script>
import logoImg from "../assets/freedomotic-logo.png";

export default {
  components: {},
  data() {
    return {
      username: "",
      password: "",
      useSSL: false,
      rememberMe: false,
      logoImg: logoImg
    };
  },
  created() {
    if (window.location.href.includes("localhost")) {
      this.username = "admin";
      this.password = "admin";
    }
  },
  methods: {
    login() {
      const payload = {
        username: this.username,
        password: this.password,
        rememberMe: this.remember
      };
      this.$store
        .dispatch("login", payload)
        .then(() => {
          this.$router.replace(this.$route.query.redirect || "/");
        })
        .catch(() => {
          this.$q.notify(this.$t('bad_login_information'))
        });
    }
  }
};
</script>

<style scoped>
#body {
  position: relative;
  height: 100%;
}

.q-card {
  width: 360px;
}
</style>
