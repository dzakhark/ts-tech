<template>
      <div class="sort-property">
        <span class="sort-property__priority">
          {{ priority }}.
        </span>
        <select
          class="sort-property__select"
          :value="sortProperty.title"
          @change="onChangeSortProperty"
        >
          <option v-for="property in propertiesForSelect" :key="property">{{ property }}</option>
        </select>
        <SortTypeButton
          class="sort-property__button"
          :order-type="sortProperty.orderType"
          @click.native="onClickOrderType"
        />
        <button
          class="sort-property__button"
          type="button"
          @click="onClickDelete"
        >
          <WindowClose :size="40" />
        </button>
      </div>
</template>

<script>
import SortTypeButton from './SortTypeButton.vue'
import WindowClose from 'vue-material-design-icons/WindowClose.vue'

import { SORT_ORDER_TYPES } from '../utils/sortOrderTypes'

const getNextOrderType = current => current === SORT_ORDER_TYPES.ASC ? SORT_ORDER_TYPES.DESC : SORT_ORDER_TYPES.ASC

export default {
  name: 'SortItem',
  components: {
    SortTypeButton,
    WindowClose
  },
  props: {
    index: {
      type: Number,
      required: true
    },
    sortProperty: {
      type: Object,
      required: true
    },
    allowedProperties: {
      type: Array,
      default: () => []
    }
  },
  computed: {
    priority () {
      return this.index + 1
    },
    propertiesForSelect () {
      return this.sortProperty.title ? [...this.allowedProperties, this.sortProperty.title] : this.allowedProperties
    }
  },
  methods: {
    onChangeSortProperty ({ target: { value } }) {
      this.$emit('change-property', { changedIndex: this.index, title: value })
    },
    onClickOrderType () {
      this.$emit('change-order-type', { changedIndex: this.index, newOrderType: getNextOrderType(this.sortProperty.orderType) })
    },
    onClickDelete () {
      this.$emit('delete', this.index)
    }
  }
}
</script>

<style lang="scss">
.sort-property {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
	background-color: #CADCE3;

  &__select {
    width: 80%;
  }

  &__button {
    background-color: transparent;
    border: 0;
  }
}
</style>
