<template>
  <div class="wedding-card">
    <div class="wedding-card__header">
      {{ fullName }} 
    </div>
    <div class="wedding-card__grey-txt">
      {{ wedding.startDate }}<template v-if="wedding.endDate"> - {{ wedding.endDate }}</template>
    </div>
  </div>
</template>

<script>
import { formatPersonName } from '@/services/formatPersonName'
import { mapGetters } from 'vuex'

export default {
  name: 'WeddingItem',
  props: {
    wedding: {
      type: Object,
      required: true
    }
  },
  computed: {
    ...mapGetters ('persons', [
      'getPersonById'
    ]),
    getPartner () {
      return this.getPersonById(this.wedding.partnerId)
    },
    fullName () {
      return formatPersonName(this.getPersonById(this.wedding.partnerId), {short: true, access: this.needHide})
    },
  }
}
</script>

<style scoped lang ="less">
.wedding-card {
  border-radius: 24px;
  background-color: #f5f5f5;
  box-shadow: 1.5px 7px 5px 1.5px rgba(0, 0, 0, 0.1), -1.5px 7px 5px -1.5px rgba(0, 0, 0, 0.2);
  width: 280px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 15px;
  margin-bottom: 15px;
  padding: 20px;
  height: auto;
  padding-left: 25px;
  font-size: 20px;
  white-space: normal;
  font-family: 'Inter', sans-serif;

  &__header {
    color: #000000;
    font-weight: 700;
  }

  &__grey-txt {
    color: #757575;
    font-weight: 700;
  }
}
</style>
