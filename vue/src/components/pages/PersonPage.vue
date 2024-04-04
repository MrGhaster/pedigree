<template>
  <PageLayout>
    <ScrollingPanel :sections="sections" ref="scrollingPanelRef" />
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
  data() {
    return {
      sections: [
        { id: 'info-section', title: 'Общая информация'},
        { id: 'parents-section', title: 'Родители', subField: []},
        { id: 'children-section', title: 'Дети', subField: []},
        { id: 'weddings-section', title: 'Брачные союзы', subField: []},
        { id: 'military-section', title: 'Военная служба', subField: []},
        { id: 'education-section', title: 'Образование', subField: []},
        { id: 'work-section', title: 'Работа', subField: []}
      ]
    }
  },
  computed: {
    ...mapGetters('persons', [
      'getPersonById'
    ]),
    person () {
      if (this.getPersonById(this.id)) {
        return this.getPersonById(this.id)
      }
      return emptyPerson()
    },
    id () {
      return this.$route.params.id
    },
    subField () {
      return {
        "parents-section": this.person.parents ? this.person.parents.map((parent, index) => ({
          id: this.generateSubFieldId("parent", index),
          title: `Родитель ${index + 1}`
        })): [],
        "children-section": this.person.children ? this.person.children.map((child, index) => ({
          id: this.generateSubFieldId("child", index),
          title: `Ребёнок ${index + 1}`
        })): [],
        "weddings-section": this.person.weddings ?this.person.weddings.map((wedding, index) => ({
          id: this.generateSubFieldId("wedding", index),
          title: `Брачный союз ${index + 1}`
        })): [],
        "military-section": this.person.militaries ? this.person.militaries.map((military, index) => ({
          id: this.generateSubFieldId("military", index),
          title: `Военная служба ${index + 1}`
        })): [],
        "education-section": this.person.educations ? this.person.educations.map((education, index) => ({
          id: this.generateSubFieldId("education", index),
          title: `Образование ${index + 1}`
        })): [],
        "work-section": this.person.works ? this.person.works.map((work, index) => ({
          id: this.generateSubFieldId("work", index),
          title: `Работа ${index + 1}`
        })): []
      }
    },
    scrollingPanel() {
      return this.$refs.scrollingPanelRef
    }
  },
  mounted() {
    const scrollingPanel = this.scrollingPanel
    this.addSubField(scrollingPanel)
  },
  methods: {
    generateSubFieldId(sectionName, index) {
      return `${sectionName}-${index}`
    },
    addSubField(scrollingPanel) {
      this.sections.forEach(section => {
        const subField = this.subField[section.id]
        if (subField && subField.length > 0) {
          subField.forEach(subField => {
            scrollingPanel.addSubField(section.id, subField)
          })
        }
      })
    }
  }
}
</script>
