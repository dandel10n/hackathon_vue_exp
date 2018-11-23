<template>
  <div>
    <!-- render homepage -->

    <h1>{{homeContent.fields.title}}</h1>
    <p class="intro">{{homeContent.fields.intro}}</p>

    <div class="c-pageBanner">
        <picture>
            <source class="c-pageBanner-img" media="(min-width: 860px)" :srcset="homeContent.fields.banner.fields.file.url + '?w=1388'" data-ft="restaurant-level-image--wide">
            <source class="c-pageBanner-img" media="(min-width: 600px)" :srcset="homeContent.fields.banner.fields.file.url + '?w=1025'" data-ft="restaurant-level-image--mid">
            <img class="c-pageBanner-img" :srcset="homeContent.fields.banner.fields.file.url + '?w=769'" alt="" data-ft="restaurant-level-image--narrow" >
        </picture>
    </div>

    <ul>
      <li v-for="(restaurant, index) in restaurants" v-bind:key="index">
        {{ restaurant.fields.restaurantName }}
      </li>
    </ul>

    <nuxt-link :to="{ name: 'slug', params: { slug: 'privacypolicy' }}">Privacy Policy</nuxt-link><br />
    <nuxt-link :to="{ name: 'slug', params: { slug: 'termsandconditions' }}">Terms and Conditions</nuxt-link>

  </div>
</template>

<script>
  import {createClient} from '~/plugins/contentful.js'

  const client = createClient()

  export default {
    // `env` is available in the context object
    asyncData ({env}) {
      return Promise.all([
        client.getEntries({
          'content_type': env.CTF_RESTAURANT_CARD_CONTENT_TYPE
        }),
        client.getEntries({
          'content_type': env.CTF_LANDING_PAGE_CONTENT_TYPE
        })
      ]).then(([restaurants, homeContent]) => {
        // return data that should be available
        // in the template
        return {
          restaurants: restaurants.items,
          homeContent: homeContent.items[0]
        }
      }).catch(console.error)
    }
  }
</script>

<style scoped>

  h1,
  .intro {
    text-align: center;
  }

  img {
    width: 100%;
  }

  .c-pageBanner {
    position: relative;
  }

  .c-pageBanner::after {
    content: "";
    display: block;
    height: 100%;
    background: url(https://dy3erx8o0a6nh.cloudfront.net/images/color-rays.svg) no-repeat bottom center;
    background-size: 101%;
    position: absolute;
    bottom: 0;
    right: 0;
    left: 0;
  }
</style>
