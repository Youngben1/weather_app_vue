<template>
    <div>
        <Suspense>
            <AsyncCityView />
            <template #fallback>
                <p>Loading ding ding Loading....</p>
            </template>
        </Suspense>
    </div>
</template>

<script setup>
import AsyncCityView from "../components/AsyncCityView.vue"
</script>
