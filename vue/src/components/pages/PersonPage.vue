<template>
  <PageLayout>
    <ScrollingPanel :sections="sections" />
    <NavigationPanel />
    <section class="p-16">
      <PersonCard :person="person" />
    </section>
  </PageLayout>
</template>

<script>
import { mapGetters } from 'vuex'
import PageLayout from '@/components/parts/PageLayout.vue'
import PersonCard from '@/components/cards/PersonCard.vue'
import NavigationPanel from '@/components/ui/NavigationPanel.vue'
import ScrollingPanel from '@/components/ui/ScrollingPanel.vue'
import { emptyPerson } from '@/services/person'

export default {
  name: 'PersonPage',
  components: {
    PageLayout,
    PersonCard,
    NavigationPanel,
    ScrollingPanel
  },
  computed: {
    ...mapGetters('persons', [
      'getPersonById'
    ]),
    sections () {
      return [
        { id: 'info-section', title: 'Общая информация', subField: this.generateSubField('info')},
        { id: 'parents-section', title: 'Родители', subField: this.generateSubField('parents') },
        { id: 'childs-section', title: 'Дети', subField: this.generateSubField('childs') },
        { id: 'weddings-section', title: 'Брачные союзы', subField: this.generateSubField('weddings') },
        { id: 'military-section', title: 'Военная служба', subField: this.generateSubField('militaries') },
        { id: 'education-section', title: 'Образование', subField: this.generateSubField('educations') },
        { id: 'work-section', title: 'Работа', subField: this.generateSubField('works') }
      ]
    },
    id () {
      return this.$route.params.id
    },
    person () {
      if (this.getPersonById(this.id)) {
        return this.getPersonById(this.id)
      }
      return emptyPerson()
    }
  },
  methods: {
    generateSubFieldId(sectionName, index) {
      return `${sectionName}-${index}`
    },
    generateSubField(sectionName) {
      if (this.person[sectionName]) {
        return this.person[sectionName].map((item, index) => ({
          id: this.generateSubFieldId(sectionName, index),
          title: `${sectionName.charAt(0).toUpperCase() + sectionName.slice(1)} ${index + 1}`
        }))
      }
      return []
    }
  }
}
</script>
