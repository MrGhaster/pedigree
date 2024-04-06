<template>
  <PageLayout>
    <ScrollingPanel :sections="sections" />
    <section class="p-16">
      <PersonForm v-model="form" />
      <SimpleButton 
        class ="person-page__btn" 
        type="primary" 
        @click="() => editPersonHandler()"
      >
        Сохранить
      </SimpleButton>
      <SimpleButton 
        class ="person-page__btn" 
        type="danger" 
        @click="() => cancel()"
      >
        Отмена
      </SimpleButton>
    </section>
  </PageLayout>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import PageLayout from '@/components/parts/PageLayout.vue'
import PersonForm from '@/components/forms/PersonForm.vue'
import { emptyPerson } from '@/services/person'
import SimpleButton from '@/components/ui/SimpleButton.vue'
import ScrollingPanel from '@/components/ui/ScrollingPanel.vue'

export default {
  name: 'EditPersonPage',
  components: {
    PageLayout,
    PersonForm,
    SimpleButton,
    ScrollingPanel
  },
  data () {
    return {
      form: emptyPerson()
    }
  },
  computed: {
    ...mapGetters('persons', [
      'getPersonById'
    ]),
    ...mapGetters('settings', [
      'getMode'
    ]),
    id () {
      return this.$route.params.id
    },
    person () {
      return this.getPersonById(this.id)
    },
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
    }
  },
  mounted () {
    if(this.getMode === 'user') { 
      this.$router.push({ name: this.$routes.HOME })
    } else {
      if (this.person) {
        this.form = {
          ...this.form,
          ...this.person
        }
      } else {
        this.$router.push({ path: '/' })
      }
    }
  },
  methods: {
    ...mapActions('persons', [
      'editPerson'
    ]),
    editPersonHandler () {
      this.editPerson(this.form)
      this.goBack()
    },
    cancel () {
      this.goBack()
    },
    goBack () {
      this.$router.go(-1)
    },
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

<style scoped lang="less">
.person-page {
  &__btn {
    margin-top: 10px;
    margin-right: 10px;
    margin-bottom: 20px;
  }
}
</style>
