<script lang="ts">
import { type PropType, defineComponent } from 'vue';

import Logo from '@/components/Logo.vue';
import SSOLogin from '@/components/SSOLogin.vue';
import type { IFormBoxConfig } from '@/Interface';

export default defineComponent({
	name: 'AuthView',
	components: {
		Logo,
		SSOLogin,
	},
	props: {
		form: {
			type: Object as PropType<IFormBoxConfig>,
		},
		formLoading: {
			type: Boolean,
			default: false,
		},
		subtitle: {
			type: String,
		},
		withSso: {
			type: Boolean,
			default: false,
		},
	},
	methods: {
		onUpdate(e: { name: string; value: string }) {
			this.$emit('update', e);
		},
		onSubmit(values: { [key: string]: string }) {
			this.$emit('submit', values);
		},
		onSecondaryClick() {
			this.$emit('secondaryClick');
		},
	},
});
</script>

<template>
	<div :class="$style.container">
		<div :class="$style.logoContainer">
			<Logo />
		</div>
		<div v-if="subtitle" :class="$style.textContainer">
			<n8n-text size="large">{{ subtitle }}</n8n-text>
		</div>
		<div :class="$style.formContainer">
			<n8n-form-box
				v-bind="form"
				data-test-id="auth-form"
				:button-loading="formLoading"
				@secondary-click="onSecondaryClick"
				@submit="onSubmit"
				@update="onUpdate"
			>
				<SSOLogin v-if="withSso" />
			</n8n-form-box>
		</div>
	</div>
</template>

<style lang="scss" module>
body {
	background-color: var(--color-background-light);
}

.container {
	display: flex;
	align-items: center;
	flex-direction: column;
	padding-top: var(--spacing-2xl);

	> * {
		margin-bottom: var(--spacing-l);
		width: 352px;
	}
}

.logoContainer {
	display: flex;
	justify-content: center;
}

.textContainer {
	text-align: center;
}

.formContainer {
	padding-bottom: var(--spacing-xl);
}
</style>

<style lang="scss">
.el-checkbox__label span {
	font-size: var(--font-size-2xs) !important;
}
</style>
