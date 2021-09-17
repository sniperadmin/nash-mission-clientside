<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <div class="text-center">
        <logo />
        <vuetify-logo />
        <img src="sanity.png" width="300" alt="sanity" class="my-auto">
      </div>
      <v-card>
        <v-card-title class="headline">
          Welcome to the Vuetify + Nuxt.js + Sanity (mini commerce)
        </v-card-title>
        <v-card-text>
          <v-row justify="center">
            <v-card
              :loading="loading"
              max-width="400"
            >
              <template slot="progress">
                <v-progress-linear
                  color="deep-purple"
                  height="10"
                  indeterminate
                ></v-progress-linear>
              </template>

              <v-img
                height="250"
                :src="product.imageUrl"
              ></v-img>

              <v-card-title>{{ product.title }}</v-card-title>

              <v-card-text>
                <v-row
                  align="center"
                  class="mx-0"
                >
                  <v-rating
                    :value="4.5"
                    color="amber"
                    dense
                    half-increments
                    readonly
                    size="14"
                  ></v-rating>

                  <div class="grey--text ms-4">
                    4.5 (413)
                  </div>
                </v-row>

                <div class="my-4 text-subtitle-1">
                  $ {{ product.price }}
                </div>

                <div>{{ product.blurb }}</div>
              </v-card-text>

              <v-divider class="mx-4"></v-divider>

              <v-card-actions>
                <v-btn
                  color="deep-purple lighten-2"
                  text
                  class="my-1"
                >
                  Reserve
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn
            color="primary"
            nuxt
            to="/inspire"
          >
            Continue
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'
import { groq } from '@nuxtjs/sanity'

export default {
  components: {
    Logo,
    VuetifyLogo
  },

  async asyncData({ params, $sanity }) {
    console.log(params.slug);
    const query = groq`{ "product":
      *[_type == "product" && slug.current == "${params.slug}"][0] {
        title,
        slug,
        _id,
        "price": defaultProductVariant.price,
        "imageUrl": defaultProductVariant.images[0].asset->url,
        "blurb": blurb.en
      }
    }`

    const { product } = await $sanity.fetch(query)

    return { product }
  },

  data: () => ({
    loading: true,
    selection: 1,
  }),

  mounted() {
    if (this.products && this.products.product) {
      this.loading = false;
    }
  }
}
</script>
