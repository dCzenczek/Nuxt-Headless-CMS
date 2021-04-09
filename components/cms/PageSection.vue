<template>
  <v-card tag="section" class="page-section mb-10" :dark="theme === 'dark'" flat tile>
    <v-container>
      <h1 class="text-center mb-5">
        {{ title }}
      </h1>

      <div
        v-for="contentBlock in contentBlocks"
        :key="contentBlock.name"
        class="mb-5"
      >
        <component
          :is="contentBlock.component"
          v-bind="contentBlock.props"
        />
      </div>
    </v-container>
  </v-card>
</template>

<script>
import { parseCMSBlock } from '@/helpers/CMS'

export default {
  name: 'PageSection',

  props: {
    title: {
      type: String,
      default: ''
    },

    content: {
      type: Array,
      default: () => []
    },

    theme: {
      type: String,
      default: 'light',
      validator: val => val === 'light' || val === 'dark'
    }
  },

  computed: {
    contentBlocks () {
      return this.content.map(parseCMSBlock)
    }
  }
}
</script>

<style lang="scss" scoped>
.page-section {
  h1 {
    font-size: 3rem;
    font-weight: 300;
  }
}
</style>
