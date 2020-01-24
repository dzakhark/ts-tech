<template>
  <form @submit.prevent="onSubmit" class="sort-form">
    <div class="sort-form__header">
      <span>Properties</span>
      <span>Order</span>
    </div>
    <div class="sort-form__body">
      <div>
        <SortItem
          class="sort-property"
          v-for="(property, idx) in selectedProperties"
          :key="idx"
          :index="idx"
          :sort-property="property"
          :allowed-properties="allowedProperties"
          @change-property="onChangeProperty"
          @change-order-type="onChangeOrderType"
          @delete="deleteProperty"
        />
      </div>
      <button
				v-if="showButton"
				class="sort-form__add-property"
				type="button"
				@click="addProperty"
			>
				<PlusCircleOutline />
				Add property
			</button>
    </div>
    <button
			class="sort-form__sort"
			type="submit"
			:disabled="disableSort"
		>
			Sort
		</button>
  </form>
</template>

<script>
import PlusCircleOutline from 'vue-material-design-icons/PlusCircleOutline.vue'
import SortItem from './SortItem'

const SORT_PROPERTIES_SHORT = ['Affiliate', 'Balance', 'Bonus balance', 'Campaign', 'Cash balance', 'Country', 'Trader points']

const transformPropertyTitle = (title = '') => title.toLowerCase().split(' ').join('_')

export default {
  name: 'SortForm',
  components: {
    SortItem,
    PlusCircleOutline
  },
  data: () => ({
    sortProperties: [...SORT_PROPERTIES_SHORT],
    selectedProperties: []
  }),
  computed: {
    showButton () {
      return !this.selectedProperties.length || this.selectedProperties.length !== this.sortProperties.length
    },
    selectedPropertiesTitleList () {
      return this.selectedProperties.map(item => item.title)
    },
    allowedProperties () {
      return this.sortProperties.filter(item => !this.selectedPropertiesTitleList.includes(item))
    },
    disableSort () {
      return !this.selectedProperties.length
    }
  },
  methods: {
    addProperty () {
      this.selectedProperties.push({
        title: this.allowedProperties[0],
        orderType: 'ASC'
      })
    },
    onChangeProperty ({ changedIndex, title }) {
      this.selectedProperties = this.selectedProperties.map(
        (item, idx) => idx !== changedIndex ? item : ({ ...item, title })
      )
    },
    onChangeOrderType ({ changedIndex, newOrderType }) {
      this.selectedProperties = this.selectedProperties.map(
        (item, idx) => idx !== changedIndex ? item : ({ ...item, orderType: newOrderType })
      )
    },
    deleteProperty (deletedIndex) {
      this.selectedProperties = this.selectedProperties.filter((item, idx) => idx !== deletedIndex)
    },
    onSubmit () {
      const result = this.selectedProperties
        .map((item, idx) => ({ ...item, name: transformPropertyTitle(item.title), priority: idx + 1 }))
      console.log(result)
      alert('Sorted')
    }
  }
}
</script>

<style lang="scss">
.sort-form {
	max-width: 600px;
	margin: 0 auto;

  &__header {
		display: flex;
		justify-content: space-between;
    padding: 10px 50px;
  }

	&__body {
		margin-bottom: 30px;
	}

	&__add-property {
		padding: 10px 15px;
		border: 1px solid #51BF5C;
		border-radius: 5px;
		background-color: transparent;
		color: #51BF5C;
	}

	&__sort {
		display: block;
		padding: 15px 25px;
		margin: 0 auto;
		border: 0;
		border-radius: 5px;
		color: #ffffff;
		background-color: #2881A4;
	}
}
</style>
