<template>
  <div class="custom-card">
    <div class="custom-card__header">
      {{ education.name }}
    </div>
    <div class="custom-card__grey-txt">
      {{ formatDate(education.startDate) }} - {{ education.endDate ? formatDate(education.endDate) : 'настоящее время' }}
    </div>
    
    <div class="custom-card__type">
      Тип:<span class="custom-card__grey-txt"> {{ education.type }} </span>
    </div>
    <div class="custom-card__type">
      Уровень образования:<span class="custom-card__grey-txt"> {{ education.level }} </span> 
    </div>
    <div class="custom-card__type">
      Город:<span class="custom-card__grey-txt"> {{ education.city }} </span> 
    </div>
  </div>
</template>

<script>
export default {
  name: 'EducationCard',
  props: {
    education: {
      type: Object,
      required: true
    }
  },
  methods: {
    formatDate(date) {
      // Функция для форматирования даты, если необходимо
      return date;
    }
  }
}
</script>
