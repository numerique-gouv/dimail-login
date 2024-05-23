<script setup lang="ts">
import '@gouvfr-lasuite/integration/dist/css/homepage-gaufre.css';

const serviceTitle = null
const serviceDescription = ''
const logoText = ['Gouvernement']

const quickLinks = []

const skipLinks = []

useHead({
  script: [
    {
      id: 'lasuite-gaufre-script',
      src: "https://integration.lasuite.numerique.gouv.fr/api/v1/gaufre.js",
      type: "text/javascript",
      defer: true,
    },
  ],
})

</script>

<template>
  <DsfrSkipLinks :links="skipLinks" />
  <LaSuiteHeader />
  <main
    id="content"
    role="main"
    tabindex="0"
  >
    <slot />
  </main>
</template>