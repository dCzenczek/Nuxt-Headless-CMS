<template>
  <section id="home-page">
    <PageHeader :title="header.title" :background-image="header.backgroundImage" />

    <template v-for="contentBlock in contentBlocks">
      <component :is="contentBlock.component" :key="contentBlock.name" v-bind="contentBlock.props" />
    </template>
  </section>
</template>

<script>
import { mapState } from 'vuex'
import CMSMixin from '@/mixins/CMSMixin'
import PageHeader from '@/components/cms/PageHeader'

export default {
  name: 'HomePage',

  components: {
    PageHeader
  },

  mixins: [CMSMixin],

  head () {
    return {
      title: this.homePage.metaTitle,
      meta: [
        { hid: 'description', name: 'description', content: this.homePage.metaDescription }
      ]
    }
  },

  computed: {
    ...mapState({
      homePage: state => state.config.homePage
    }),

    pageHeader () {
      return this.homePage.header
    },

    content () {
      return this.homePage.sections
    }
  }
}
</script>
