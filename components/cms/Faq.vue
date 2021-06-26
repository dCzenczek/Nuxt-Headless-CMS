<template>
  <div class="timeline">
    <v-expansion-panels focusable inset>
      <v-expansion-panel
        v-for="item in items"
        :key="item.name"
      >
        <v-expansion-panel-header
          color="primary"
          class="faq-question"
        >
          {{ item.question }}
        </v-expansion-panel-header>
        <v-expansion-panel-content
          class="faq-answer"
        >
          {{ item.answer }}
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
  </div>
</template>

<script>

export default {
  name: 'Faq',

  components: {},

  props: {
    title: {
      type: String,
      default: ''
    },
    items: {
      type: Array,
      default: () => []
    }
  }
}
</script>

<style lang="scss" scoped>

.faq-answer {
  padding-top: 10px;
}

.faq-question::before {
  opacity: 0 !important;
}
</style>
