<template>
  <main>
    <div class="section">
      <div class="container">
        <Person v-if="person" :person="person"/>
        <div v-else>Désolé, cette page n'existe pas ou a été dépubliée. 😭</div>
      </div>
    </div>
  </main>
</template>

<script>
import Person from '@/components/Person'
import { getPersonBySlug } from '@/services/content'
import { generateSocialShareHeadersMeta } from '@/services/helpers'

export default {
  components: { Person },
  computed: {
    person() {
      return getPersonBySlug(this.$route.params.slug)
    }
  },
  head() {
    if (!this.person) {
      return {}
    }
    const title = `Profil de ${this.person.prenom} ${
      this.person.nom
    }, développeur freelance ${this.person.technologies.join(', ')}`
    const description = process.env.POPCORN_OG_DEFAULT_DESCRIPTION
    const image = `${process.env.POPCORN_BASE_URL}${this.person.photo}`
    const url = `${process.env.POPCORN_BASE_URL}${this.$route.path}`
    return {
      meta: [
        ...generateSocialShareHeadersMeta({ title, description, image, url })
      ]
    }
  }
}
</script>
