<template>
    <div
    :class="[isError ? 'bg-red-500' : 'bg-green-500']"
    class="text-white text-center uppercase font-bold p-3 mb-3 rounded-md"
    >
        {{ alert.msg }}
    </div>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
    alert: {
        type: Object,
        required: true,
    }
})

const isError = computed(() => {
    return props.alert.type === 'error';
})
</script>