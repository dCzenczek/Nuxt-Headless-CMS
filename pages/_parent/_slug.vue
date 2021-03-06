<template>
  <article>
    <PageHeader
      :title="header.title"
      :background-image="header.backgroundImage"
      :background-color="header.backgroundColor"
    />

    <component
      :is="contentBlock.component"
      v-for="contentBlock in contentBlocks"
      :key="contentBlock.name"
      v-bind="contentBlock.props"
    />
  </article>
</template>

<script>
import config from '@/nuxt.config'
import { createClient } from '@/plugins/contentful'
import PageMixin from '@/mixins/PageMixin'

import PageHeader from '@/components/cms/PageHeader'

export default {
  name: 'CMSPage',

  components: {
    PageHeader
  },

  mixins: [PageMixin],

  async asyncData ({ app, env, params, error }) {
    const client = createClient()

    const entries = await client.getEntries({
      content_type: env.pageContentModel,
      locale: app.i18n.localeProperties.code,
      include: env.contentfulIncludeLevel
    })

    const page = entries.items.find(entry => entry.fields.slug === params.slug && entry.fields.parentPage.fields.slug === params.parent)

    if (!page) { error({ statusCode: 404 }) }

    return {
      metaTitle: page?.fields?.metaTitle,
      metaDescription: page?.fields?.metaDescription,
      pageHeader: page?.fields?.header,
      content: page?.fields?.sections
    }
  },

  head () {
    return {
      title: this.metaTitle ? this.metaTitle : config.head.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.metaDescription || config.head.meta.find(el => el.hid === 'description').content
        }
      ]
    }
  }
}
</script>
