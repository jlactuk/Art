<template>
    <article  :class="['py-8 px-4 border rounded flex flex-col gap-2 relative', props.info.id % 2 === 0 ? 'bg-red-400' : 'bg-green-400']">
     <p class="text-lg font-bold absolute top-2 left-2">id: {{ props.info.id }}</p>
          <h3 class="text-2xl font-bold">title: {{ props.info.title }}</h3>
          <p>{{ props.info.body }}</p>
        </article>
</template>

<script lang="ts" setup>

    const props = defineProps({info : {
        type: Object,
        default(rawProprs) {
            return {
                id: rawProprs.id,
                title: rawProprs.title,
                body: rawProprs.body,
                userId: rawProprs.userId,
            }
        }
    }})

</script>