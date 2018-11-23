<template lang="html">
  <div>
    <h1>{{staticContent.fields.title}}</h1>
    <div>
      {{staticContent.fields.content}}
    </div>

  </div>
</template>

<script>
  import {createClient} from '~/plugins/contentful.js'

  const client = createClient()

  export default {
    // `env` is available in the context object
    asyncData ({env, params}) {
      console.log(params.slug);
      return Promise.all([
        client.getEntries({
          'content_type': env.CTF_STATIC_PAGE_CONTENT_TYPE,
          'fields.slug': params.slug,
        })
      ]).then(([staticContent]) => {
        // return data that should be available
        // in the template
        console.log(staticContent.items);
        return {
          staticContent: staticContent.items[0]
        }
      }).catch(console.error)
    }
  }
</script>


<style lang="css">
</style>
