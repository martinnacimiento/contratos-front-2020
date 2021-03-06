<template lang="pug">
  div
    v-snackbar(v-model="snackbar" :color="color") {{ message }}
    v-dialog(
      v-model="dialog"
      @click:outside="resetDialog"
      width="500"
    )
      v-card(class="rounded-lg")
        v-card-title
        v-card-text Estas seguro que desea eliminar el objeto {{item.object}}?
        v-card-actions
          v-spacer
          v-btn(text @click="resetDialog") Cancelar
          v-btn(text @click="destroy(item.id)") Eliminar
    v-card(class="rounded-lg" :elevation="12")
      v-data-table(
        :headers='headers'
        :items='objects'
        :search="search"
        :loading="$fetchState.pending"
        sort-by='id'
      )
        template( v-slot:top)
          v-toolbar(flat)
            v-toolbar-title Objetos
            v-divider(inset vertical).mx-4
            v-text-field(v-model="search" append-icon="mdi-table-search" label="Buscar" single-line hide-details)
            v-spacer
            v-btn(v-if="can('create.object')" color="primary" small :to="{name: 'objetos-nuevo'}")
              v-icon mdi-plus-circle-outline
        template(v-if="can('edit.object') || can('destroy.object')" v-slot:item.actions="{ item }")
          v-icon(v-if="can('edit.object')" small @click="$router.push({name: 'objetos-id', params: { id: item.id}})").mr-2 mdi-pencil
          v-icon(v-if="can('destroy.object')" small @click="confirmation(item)").mr-2 mdi-delete
</template>
<script>
import { mapGetters } from 'vuex'
export default {
  async fetch() {
    const { objects } = await this.$axios.$get('objetos')
    this.objects = objects
  },
  data: () => ({
    objects: [],
    headers: [
      { text: 'ID', value: 'id' },
      { text: 'Objeto', value: 'object' },
      {
        text: 'Acciones',
        align: 'right',
        value: 'actions',
        sortable: false,
        filterable: false,
      },
    ],
    dialog: false,
    item: { object: '' },
    message: '',
    snackbar: false,
    color: 'green',
    search: '',
  }),
  computed: {
    ...mapGetters(['can']),
  },
  methods: {
    async get() {
      const { objects } = await this.$axios.$get(`objetos`)
      this.objects = objects
    },
    confirmation(item) {
      this.item = item
      this.dialog = true
    },
    resetDialog() {
      this.item = { sex: '' }
      this.dialog = false
    },
    async destroy(id) {
      await this.$axios.$delete(`objetos/${id}`)
      this.get()
      this.resetDialog()
      this.snack('El objeto ha sido eliminado.')
    },
    snack(message, color = 'green') {
      this.color = color
      this.message = message
      this.snackbar = true
    },
  },
}
</script>
