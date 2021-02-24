<template>
  <section>
    {{ metaTitle }}
    {{ metaDescription }}
    {{ entries }}
  </section>
</template>

<script>
import config from '@/nuxt.config'

export default {
  name: 'CMSPage',

  async asyncData ({ app, $axios, route, error }) {
    const url = `/spaces/${process.env.contentfulSpace}/environments/${process.env.contentfulEnv}/entries?` +
      `content_type=page&fields.slug=${route.params.slug}` +
      `&access_token=${process.env.contentfulToken}` +
      `&locale=${app.i18n.localeProperties.iso}`

    const { data } = await $axios.get(url)

    if (data.total <= 0) { error({ statusCode: 404, message: 'Page not found' }) }

    return {
      metaTitle: data.items[0]?.fields?.metaTitle,
      metaDescription: data.items[0]?.fields?.metaDescription,
      entries: data.includes?.Entry
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
