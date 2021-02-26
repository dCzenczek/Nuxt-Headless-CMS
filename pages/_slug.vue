<template>
  <section>
    {{ metaTitle }}
    {{ metaDescription }}
    {{ entries }}
  </section>
</template>

<script>
import config from '@/nuxt.config'
import { createClient } from '@/plugins/contentful'

export default {
  name: 'CMSPage',

  async asyncData ({ app, env, error }) {
    const client = createClient()

    const entries = await client.getEntries({
      content_type: env.pageContentModel,
      locale: app.i18n.localeProperties.iso
    })

    if (!entries.total) { error({ statusCode: 404, message: 'Page not found' }) }

    return {
      metaTitle: entries.items[0]?.fields?.metaTitle,
      metaDescription: entries.items[0]?.fields?.metaDescription,
      entries: entries.includes?.Entry
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
