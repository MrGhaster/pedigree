<template>
  <div class="scrolling-menu">
    <div v-for="(section, index) in sections" :key="index">
      <button class="scrolling-menu__btn" @click="() => scrollToSection(section.id)">{{ section.title }}</button>
      <div v-for="(subField, index) in section.subField" :key="index">
        <div class="scrolling-menu__sub_field" @click="() => scrollToSection(subField.id)">{{subField.title}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ScrollingForm',
  props: {
    sections: {
      type: Array,
      required: true
    }
  },
  methods: {
    scrollToSection (sectionId) {
      const element = document.getElementById(sectionId)
      if (element) {
        element.scrollIntoView({ behavior: 'smooth' })
      }
    },
    addSubField(sectionId, subField) {
      const section = this.sections.find(sec => sec.id === sectionId)
      if (section) {
        section.subField.push(subField)
      }
    }
  }
}
</script>

<style scoped lang="less">
.scrolling-menu {
  position: fixed;
  top: 50px;
  left: 10px; 

  &__btn {
    background-color: #45475F00;
    border: none;
    padding: 8px 12px;
    cursor: pointer;
    font-weight: 600;
    font-size:16px;
  }

  &__sub_field {
    background-color: #45475F00;
    color: #1c1c1c;
    border: none;
    padding: 8px 12px 8px 20px;
    cursor: pointer;
    font-weight: 550;
    font-size:14px;
  }
}

@media (max-width: 720px) {
  .scrolling-menu {
    display: none;
  }
} 
</style>
