<template>
	<div id="sc-page-wrapper">
		<div id="sc-page-content">
			<ScCard>
				<ScCardBody>
					<form class="sc-padding">
						<div class="uk-child-width-1-3@m uk-grid" data-uk-grid>
							<ScInput
								v-model="keyword"
								mode="outline"
							>
							</ScInput>
						</div>
					</form>
					<VueGoodTable
						:columns="columns"
						:rows="users"
						:pagination-options="{ enabled: true }"
						style-class="uk-table uk-table-divider scutum-vgt"
					>
					</VueGoodTable>
				</ScCardBody>
			</ScCard>
		</div>
	</div>
</template>
<script>
import 'vue-good-table/dist/vue-good-table.css'
import { VueGoodTable } from 'vue-good-table'
import ScInput from '~/components/Input'
import axios from 'axios'
export default {
    	components: {
		VueGoodTable,
		ScInput
	},
	filters: {
		stringify (value) {
			return JSON.stringify(value, null, 2)
		}
	},
	data () {
		return {
			keyword: '',
			users: []
		}
	},
	computed:{
		columns () {
			return [
				{
					label: 'Id',
					field: 'id',
					sortable: true,
					
				},
				{
					label: 'First Name',
					field: 'first_name',
				},
				{
					label: 'Last Name',
					field: 'last_name',
				}, ]
		}
	},
	watch: {
		// When the query value changes, fetch new results from
		// the API - in practice this action should be debounced
		keyword (newQuery) {
			
			axios.get(`http://demo.com/json/web/index.php?r=details%2Fview&id=${newQuery}`)
			// axios.get(`https://api.github.com/search/users?q=${newQuery}`)
				.then((res) => {
					this.users = res.data
					console.log(res)
				})
		}
	},
}
</script>