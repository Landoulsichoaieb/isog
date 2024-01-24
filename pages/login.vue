<!-- ❗Errors in the form are set on line 60 -->
<script setup>
const { signIn, data: sessionData } = useAuth()




import loginbackground from '@images/background/login-background.png';
import loginlogo from '@images/logo/brand.svg';
import loginfooter from '@images/logo/Groupe1423.svg';
import { VForm } from 'vuetify/components/VForm';

const loginbackgroundimg = loginbackground;
const loginlogoimg = loginlogo;
const loginfooterimg = loginfooter;

definePageMeta({
  layout: 'blank',
  unauthenticatedOnly: true,

})

const isPasswordVisible = ref(false)
const route = useRoute()

const ability = useAbility()

const errors = ref({
  email: undefined,
  password: undefined,
})

const refVForm = ref()

const credentials = ref({
  email: 'admin@demo.com',
  password: 'admin',
})

const rememberMe = ref(false)

async function login() {
  const response = await signIn('credentials', {
    callbackUrl: '/',
    redirect: false,
    ...credentials.value,
  })

  // If error is not null => Error is occurred
  if (response && response.error) {
    const apiStringifiedError = response.error
    const apiError = JSON.parse(apiStringifiedError)

    errors.value = apiError.data

    // If err => Don't execute further
    return
  }

  // Reset error on successful login
  errors.value = {}

  // Update user abilities
  const { user } = sessionData.value

  useCookie('userData').value = user

  // Save user abilities in cookie so we can retrieve it back on refresh
  useCookie('userAbilityRules').value = user.abilityRules

  ability.update(user.abilityRules ?? [])

  navigateTo(route.query.to ? String(route.query.to) : '/', { replace: true })
}

const onSubmit = () => {
  refVForm.value?.validate().then(({ valid: isValid }) => {
    if (isValid)
      login()
  })
}
</script>

<template>
  <VRow
    no-gutters
    class="auth-wrapper bg-surface"
  >
    <VCol
    cols="12"
      lg="6"
      class="d-none d-lg-flex"
      :style="{ backgroundImage: `url(${loginbackgroundimg})` }"
    >
    </VCol>

    <VCol
      cols="12"
      lg="6"
      class="auth-card-v2 d-flex flex-column align-center justify-center pt-16"
      style="background-color: #133155;"
    >

    <div class="position-relative rounded-lg ma-8" style="width: 292px;height: auto;">
      <img
        :src="loginlogoimg" 
        alt="Description of the image"
        class="img-fluid colorized-svg"
      />
    </div>
      <VCard
        flat
        :max-width="350"
        class="mt-12 mt-sm-0 pa-4"
        style="background-color: #133155;"
      >
        <VCardText>

          <center>
          <h4 class="text-h4 mb-1 text-white geomanist-regular">
            Connectez-vous
          </h4>
          <p class="mb-0 text-white geomanist-light">
            Pour accéder à votre espace
          </p>
        </center>
        </VCardText>
        <VCardText>
          <VForm
            ref="refVForm"
            @submit.prevent="onSubmit"
          >
            <VRow>
              <!-- email -->
              <VCol cols="12" class="pa-0 mt-8 geomanist-book">
                <AppTextField class="AppTextField input-log"
                  v-model="credentials.email"
                  placeholder="johndoe@email.com"
                  type="email"
                  autofocus
                  :rules="[requiredValidator, emailValidator]"
                  :error-messages="errors.email"
                />
              </VCol>

              <!-- password -->
              <VCol cols="12" class="pa-0 mt-3 geomanist-book">
                <AppTextField class="AppTextField input-log"
                  v-model="credentials.password"
                  :height="49"
                  placeholder="············"
                  :rules="[requiredValidator]"
                  :type="isPasswordVisible ? 'text' : 'password'"
                  :error-messages="errors.password"
                  :append-inner-icon="isPasswordVisible ? 'tabler-eye-off' : 'tabler-eye'"
                  @click:append-inner="isPasswordVisible = !isPasswordVisible"
                />

                <VBtn
                  type="submit"
                  class="text-uppercase mt-3 geomanist-book button-log"
                >
                  se connecter
                </VBtn>

                <div class="d-flex align-center flex-wrap justify-center mt-1 mb-4 geomanist-light">
                  <NuxtLink
                    class="ms-2 mb-1 text-white mt-3"
                    :to="{ name: 'forgot-password' }"
                    style="font-size: 12px;"
                  >
                    Mot de passe oubliée?
                  </NuxtLink>
                
                </div>
              </VCol>
            </VRow>
          </VForm>
        </VCardText>
        
      </VCard>
      <div class="text-white text-center mt-3 mb-3 mt-auto" style="width:100%;">
        <hr class="mx-16" style="border-color:#97A4B4;"/>
        <div class="mx-12 my-3">
          <div class="d-flex justify-center align-center flex-wrap geomanist-light" style="font-size: 16px;">
            <NuxtLink class="ms-2 mb-1 text-white mt-2 mx-5" :to="{ name: 'forgot-password' }" >
              Conditions générales d'utilisation
            </NuxtLink>
            <NuxtLink class="ms-2 mb-1 text-white mt-2 mx-5" :to="{ name: 'forgot-password' }">
              Mentions légales
            </NuxtLink>

            <NuxtLink class="ms-2 mb-1 text-white mt-2 mx-5" :to="{ name: 'forgot-password' }">
              Politique de cookies
            </NuxtLink>
            <NuxtLink class="ms-2 mb-1 text-white mt-2 mx-5" :to="{ name: 'forgot-password' }">
              Politique de protection des données à caractère personnel
            </NuxtLink>
          </div>
          <div class="mx-1 mt-4 d-flex justify-center align-center flex-wrap gothamlight" style="font-size: 12px;">
            © 2023 . All Rights Reserved . Powered by xenius consulting with
            <img
              :src="loginfooterimg" 
              alt="Description of the image"
              class="img-fluid style-svg-cliniq"
            />
          </div>
        </div>
      </div>
    </VCol>
    
  </VRow>
</template>

<style lang="scss">
@use "@core/scss/template/pages/page-auth.scss";
</style>
