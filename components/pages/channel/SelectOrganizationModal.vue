<template>
	<modal
		name="modal-select-organization"
		:draggable="$device.isDesktop"
		:adaptive="true"
		:reset="true"
		width="50%"
		height="auto"
		@before-open="beforeOpen"
		@before-close="beforeClose"
	>
		<div class="uk-card uk-form-horizontal">
			<div class="uk-text-right">
				<button
					type="button"
					class="close-button"
					@click="hide()"
				>
					<span class="mdi mdi-close"></span>
				</button>
			</div>
			<div class="vm--title uk-card-title">
				簡易チャンネルを開設する組織を選択してください
			</div>
			<ScCardBody>
				<VueGoodTable
					class="overout45"
					@on-selected-rows-change="selectionChanged"
					:columns="modalColumns"
					:rows="organization_dtlists"
					:fixed-header="true"
					:pagination-options="{
						mode: 'pages',
						enabled: false,
						rowsPerPageLabel: '表示件数',
						nextLabel: '次へ',
						prevLabel: '前へ',
						pageLabel: '',
						ofLabel: 'ページ目を表示中 / ',
						dropdownAllowAll: false,
					}"
					style-class="uk-table uk-table-divider scutum-vgt uk-table-middle"
					:sort-options="{
						enabled: true,
					}"
					:select-options="{
						enabled: true,
						selectOnCheckboxOnly: true, // only select when checkbox is clicked instead of the row
						selectionInfoClass: 'custom-class',
						selectionText: 'rows selected',
						clearSelectionText: 'clear',
						disableSelectInfo: true, // disable the select info panel on top
						selectAllByGroup: true, // when used in combination with a grouped table, add a checkbox in the header row to check/uncheck the entire group
					}"
				>
					<template slot="table-row" slot-scope="props">
						<span v-if="props.column.field === 'organization_type'">
							{{ props.formattedRow[props.column.field] | view(props.column.field) }}
						</span>
						<template v-else>
							{{ props.formattedRow[props.column.field] }}
						</template>
					</template>
				</VueGoodTable>
				<br>
				<input
					class="font12 sc-button md-color-white sc-button-primary sc-button-small min_w80 waves-effect waves-button"
					type="submit"
					value="選択"
					@click="click()"
				>
			</ScCardBody>
		</div>
	</modal>
</template>
<script>
import { CONST } from '~/const.js';
import 'vue-good-table/dist/vue-good-table.css'
import { VueGoodTable } from 'vue-good-table';
export default {
	components: {
		VueGoodTable,
	},
	data: () => ({
		organization_dtlists: null,
		organization_id: []
	}),
	computed: {
		modalColumns () {
			return [
				{
					label: '組織名',
					field: 'organization_name',
					sortable: true,
				},
				{
					label: '組織名フリガナ',
					field: 'organization_name_kana',
					sortable: true,
				},
				{
					label: '組織タイプ',
					field: 'organization_type',
					sortable: true,
				},
			]
		},
	},

	methods: {
		click () {
			this.hide();
			this.$emit('next', this.organization_id)
		},
		selectionChanged (params) {
			this.organization_id = _.map(params.selectedRows, 'organization_id');
		},
		hide () {
			this.$modal.hide('modal-select-organization')
		},
		beforeOpen (event) {
			this.organization_dtlists = event.params;
		},
		beforeClose (event) {
		},
	},
}
</script>
<style lang="scss">
	@import '~scss/plugins/vue-good-table.scss';
</style>
