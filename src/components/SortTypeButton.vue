<template>
	<button type="button">
		<SortAscending v-if="isASCOrderType" />
		<SortDescending v-else />
	</button>
</template>

<script>
import SortAscending from 'vue-material-design-icons/SortAscending.vue'
import SortDescending from 'vue-material-design-icons/SortDescending.vue'

import { SORT_ORDER_TYPES } from '../utils/sortOrderTypes'

export default {
  name: 'SortTypeButton',
  components: {
    SortAscending,
    SortDescending
  },
  props: {
    orderType: {
      type: String,
      default: SORT_ORDER_TYPES.ASC
    }
  },
  computed: {
    isASCOrderType () {
      return this.orderType === SORT_ORDER_TYPES.ASC
    }
  }
}
</script>
