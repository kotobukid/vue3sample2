<template lang="pug">
img(alt="Vue logo" src="./assets/logo.png")
br
table
    tr
        td(@click="setInnerContent") Welcome to Your Vue.js + TypeScript App
    tr
        td(@click="setInnerContent") Aloha!
br
HelloWorld(:msg="inner_content")
</template>

<script lang="ts">
import {defineComponent, ref, Ref} from 'vue';
import HelloWorld from './components/HelloWorld.vue';

export default defineComponent({
    name: 'App',
    components: {
        HelloWorld
    },
    setup(props: any) {
        const inner_content: Ref<string> = ref('')

        const setInnerContent = (e: PointerEvent) => {
            // @ts-ignore
            inner_content.value = e!.target!.innerHTML || '';
        }

        return {
            inner_content,
            setInnerContent
        }
    }
});
</script>

<style lang="less">
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

table {
    border-collapse: collapse;
}

th, td {
    border: 1px solid grey;
    padding: 4px;


    cursor: pointer;
}
</style>
