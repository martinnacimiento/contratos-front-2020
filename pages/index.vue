<template lang="pug">
  v-parallax(dark src='https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg')
    v-row(align='center' justify='center')
      v-col.text-center(cols='12')
        h1.display-1.font-weight-thin.mb-4 Contratos
        h4.subheading Crea contratos hoy!
        h4.subheading Pruebe con el nombre de usuario y contraseña: guest !
</template>

<script>
import Logo from '@/components/Logo'
export default {
  components: { Logo },
}
</script>
