<script setup>
import AuthenticatedLayout from '@/BreezeLayouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/inertia-vue3';

</script>

<template>

<Head :title="$t('Dashboard')" />

    <AuthenticatedLayout>
        <div class="py-12">
            <div class="container mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">{{ $t("You're logged in!") }}</div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
