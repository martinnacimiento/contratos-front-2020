<template lang="pug">
  div
    v-snackbar(v-model="snackbar" :color="color") {{ message }}
    v-card(class="rounded-lg" :elevation="12")
      v-card-title Nuevo sexo
      v-card-text
        v-form(ref="form" v-model="valid" lazy-validation)
          v-row
            v-col(cols="12" md="6")
              v-text-field(
                label="Sexo"
                v-model="sex.sex"
                :rules="[rules.required, rules.counter]"
              )
      v-card-actions
        v-spacer
        v-btn(outlined @click="$router.go(-1)") Volver
        v-btn(outlined @click="save" :loading="loading") Guardar
</template>
<script>
export default {
  name: 'SexosNuevo',
  data: () => ({
    valid: false,
    snackbar: false,
    color: 'green',
    message: '',
    rules: {
      required: (value) => !!value || 'Requerido.',
      counter: (value) => value?.length <= 10 || '10 caracteres máximo.',
    },
    sex: { sex: '' },
    loading: false,
  }),
  methods: {
    async save() {
      if (this.$refs.form.validate()) {
        this.loading = true
        try {
          const { message } = await this.$axios.$post(`sexos`, {
            sex: this.sex.sex,
          })
          this.snack(message)
          this.$refs.form.reset()
        } catch (error) {
          this.snack(error.response.data.message, 'error')
        }
        this.loading = false
      }
    },
    snack(message, color = 'green') {
      this.color = color
      this.message = message
      this.snackbar = true
    },
  },
}
</script>
