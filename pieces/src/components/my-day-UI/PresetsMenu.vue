<template>
    <div>
        <el-card style="max-width: 480px">
            <template #header>
            <div class="card-header">
                <span>要翻译Preset</span>
            </div>
            </template>
            <p v-for="o in 4" :key="o" class="text item">{{ 'List item ' + o }}</p>
            <template #footer>Footer content</template>
        </el-card>
    </div>
</template>

<script setup lang="ts">

</script>

<style scoped>

</style>