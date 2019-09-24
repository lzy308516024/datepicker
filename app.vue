<template>
	<div>
		<DatePicker v-model = "now"></DatePicker>
	</div>
</template>
<script>
	import DatePicker  from './date-picker';
	export default {
		data(){
			return {
				now:new Date()
			}
		},
		components:{
			DatePicker
		}
	}
</script>
<style  scoped>

</style>