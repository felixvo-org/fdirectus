<template>
	<ValueNull v-if="!displayedTranslation && !hideDisplayText" />
	<div v-else class="translation-strings-display">
		<span v-if="!hideDisplayText" class="translation-display-text">{{ displayedTranslation.translation }}</span>
		<v-menu class="menu" show-arrow :disabled="!translations || translations.length === 0">
			<template #activator="{ toggle, deactivate, active }">
				<v-icon
					v-tooltip.bottom="translations && translations.length === 0 && t('no_translations')"
					:small="!hideDisplayText"
					class="icon"
					:class="{ active }"
					name="info"
					tabindex="-1"
					@click.stop="toggle"
					@blur="deactivate"
				></v-icon>
			</template>

			<v-list class="translations">
				<v-list-item v-for="item in translations" :key="item.language">
					<v-list-item-content>
						<div class="header">
							<div class="lang">
								<v-icon name="translate" small />
								{{ item.language }}
							</div>
						</div>
						<ValueNull v-if="!item.translation" />
						<div v-else class="translation-item-text">{{ item.translation }}</div>
					</v-list-item-content>
				</v-list-item>
			</v-list>
		</v-menu>
	</div>
</template>

<script lang="ts" setup>
import { computed } from 'vue';
import { useI18n } from 'vue-i18n';
import { useUserStore } from '@/stores';
import ValueNull from '@/views/private/components/value-null';
import { TranslationString } from '@/composables/use-translation-strings';

interface Props {
	translations?: TranslationString['translations'];
	hideDisplayText?: boolean;
}

const props = withDefaults(defineProps<Props>(), { translations: () => null, hideDisplayText: false });

const { t } = useI18n();

const { currentUser } = useUserStore();

const displayedTranslation = computed(() => {
	if (!props.translations || props.translations.length === 0) return null;

	if (currentUser && 'id' in currentUser) {
		return props.translations.find((val) => val.language === currentUser.language) ?? props.translations[0];
	}

	return props.translations[0];
});
</script>

<style lang="scss" scoped>
.v-list {
	width: 300px;
}
.translation-strings-display {
	display: flex;
	align-items: center;

	.icon {
		color: var(--foreground-subdued);
		opacity: 0;
		transition: opacity var(--fast) var(--transition);
	}

	&:hover .icon,
	.icon.active {
		opacity: 1;
	}
}

.translation-display-text {
	margin-right: 4px;
	padding: 2px 0;
}

.translation-item-text {
	padding-top: 2px;
}
.translation-display-text,
.translation-item-text {
	overflow: hidden;
	white-space: nowrap;
	text-overflow: ellipsis;
}

.header {
	display: flex;
	gap: 20px;
	align-items: center;
	justify-content: space-between;
	color: var(--foreground-subdued);
	font-size: 12px;

	.lang {
		font-weight: 600;
	}

	.v-icon {
		margin-right: 4px;
	}

	.v-progress-linear {
		flex: 1;
		width: unset;
		max-width: 100px;
		border-radius: 4px;
	}
}

.v-list-item-content {
	padding-top: 4px;
	padding-bottom: 2px;
}

.v-list-item:not(:first-child) {
	.header {
		padding-top: 8px;
		border-top: var(--border-width) solid var(--border-subdued);
	}
}
</style>
