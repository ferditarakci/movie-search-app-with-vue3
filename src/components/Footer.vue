<template>
	<footer class="row mt-5">
		<div class="col-12">
			<div class="border-top mb-4"></div>
		</div>
		<div class="col-12 col-md-6">
			{{ copyright }}
		</div>
		<div class="col-12 col-md-6 text-md-right">
			<a :href="link" target="_blank">{{ linkText }}</a>
		</div>
	</footer>
</template>

<script>
export default {
	name: 'Footer',
	props: ['copyright', 'link', 'linkText'],
}
</script>
