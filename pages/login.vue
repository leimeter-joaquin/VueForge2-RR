<script setup>
const alerts = useAlertsStore()

const { auth } = useDeskree()
definePageMeta({
  layout: 'form-focus',
})

const form = reactive({
  email: '',
  password: '',
})

const loading = ref(false)
async function handleLogin() {
  loading.value = true
  try {
    await auth.login(form)
    useRouter().push('/')
  }
  catch (err) {
    alerts.error(
      'Error logging in. Please ensure email and password are correct',
    )
  }
  finally {
    loading.value = false
  }
}
</script>

<template>
  <div>
    <h2 class="card-title mb-5">
      Login
    </h2>
    <FormKit
      v-model="form"
      type="form"
      :config="{ validationVisibility: 'submit' }"
      :actions="false"
      @submit="handleLogin"
    >
      <FormKit
        type="text"
        label="Email"
        name="email"
        validation="required|email"
      />

      <FormKit
        type="password"
        name="password"
        label="Password"
        validation="required"
      />
      <AppButton class="btn-primary" :loading="loading">
        Login
      </AppButton>
    </FormKit>
  </div>
</template>
