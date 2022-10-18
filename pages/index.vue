<template>
<div>
    <HeaderVue></HeaderVue>
    <main class="container">
        <Form></Form>
    </main>
    <footer class="p-3 text-center">
        ISL @ 2022
    </footer>
</div>
</template>

<script lang="ts">
import Vue from 'vue';
import HeaderVue from '~/components/Header.vue';
import Form from '~/components/Form.vue';

export default Vue.extend({
    name: 'IndexPage',
    components: {
        HeaderVue,
        Form,
    }
})
</script>
