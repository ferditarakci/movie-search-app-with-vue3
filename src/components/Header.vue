<template>
	<header class="row">
		<div class="col-12">
			<h1 class="mb-0 pb-2">{{ title }}</h1>
			<div class="border-top mb-4"></div>
		</div>
	</header>
</template>

<script>
export default {
	name: "Header",
	props: ["title"]
}
</script>
