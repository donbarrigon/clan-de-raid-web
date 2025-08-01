<template>
    <component
        :is="isLink ? 'a' : 'button'"
        :href="isLink ? href : null"
        :class="['btn', variantClass]"
        v-bind="$attrs"
    >
        <slot />
    </component>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
    href: String,
    variant: {
        type: String,
        default: 'primary', // primary, secondary, danger, etc.
    },
})

const isLink = computed(() => !!props.href)

const variantClass = computed(() => {
    return `btn--${props.variant}`
})
</script>

<style scoped>
.btn {
    padding: 0.5rem 1rem;
    font-weight: 600;
    font-size: 0.9rem;
    border: none;
    border-radius: 0.375rem;
    cursor: pointer;
    transition: background-color 0.2s ease, border-color 0.2s ease, box-shadow 0.2s ease;
    text-decoration: none;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    outline: none;
}
.btn:focus {
    box-shadow: 0 0 0 3px var(--color-shadow);
}

/* primary */
.btn--primary {
    background-color: var(--color-primary);
    color: var(--color-primary-text);
}
.btn--primary:hover {
    background-color: var(--color-primary-hover);
}
.btn--primary:active {
    background-color: var(--color-primary-active);
}

/* secondary */
.btn--secondary {
    background-color: var(--color-surface);
    color: var(--color-secondary-text);
    border: 1px solid var(--color-secondary);
}
.btn--secondary:hover {
    background-color: var(--color-surface-alt);
    border-color: var(--color-secondary-hover);
}
.btn--secondary:active {
    background-color: var(--color-surface-alt);
    border-color: var(--color-secondary-active);
}

/* success */
.btn--success {
    background-color: var(--color-success);
    color: var(--color-text-inverted);
}
.btn--success:hover {
    background-color: var(--color-success-hover);
}
.btn--success:active {
    background-color: var(--color-success-hover);
}

/* warning */
.btn--warning {
    background-color: var(--color-warning);
    color: var(--color-text-inverted);
}
.btn--warning:hover {
    background-color: var(--color-warning-hover);
}
.btn--warning:active {
    background-color: var(--color-warning-hover);
}

/* danger */
.btn--danger {
    background-color: var(--color-danger);
    color: var(--color-text-inverted);
}
.btn--danger:hover {
    background-color: var(--color-danger-hover);
}
.btn--danger:active {
    background-color: var(--color-danger-hover);
}

/* info */
.btn--info {
    background-color: var(--color-info);
    color: var(--color-text-inverted);
}
.btn--info:hover {
    background-color: var(--color-info-hover);
}
.btn--info:active {
    background-color: var(--color-info-hover);
}
</style>
