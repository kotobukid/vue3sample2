<template lang="pug">
.hello
    h1 {{ msg }}

    button.button(@click="plus1_each") +1 each
    button.button(@click="append") append 0
    ul
        li.number(v-for="(n, $index) in numbers" @click="increment_single($index)") {{ n }}
</template>

<script lang="ts">
import {defineComponent, toRefs, ref} from 'vue';

export default defineComponent({
    props: {
        msg: {type: String, required: true}
    },
    setup(props: { msg: string }) {
        const {msg} = toRefs(props)
        const numbers = ref([0, 0, 0, 0, 0])

        const increment_single = (index: number) => {
            numbers.value[index] = numbers.value[index] + 1
        }

        const plus1_each = () => {
            numbers.value = numbers.value.map(n => n + 1)
        }

        const append = () => {
            // numbers.value = [...numbers.value, 0]
            numbers.value.push(0)
        }

        return {
            numbers,
            msg,
            append,
            increment_single,
            plus1_each
        }
    }
})
</script>

<style scoped lang="less">
h3 {
    margin: 40px 0 0;
}

ul {
    list-style-type: none;
    padding: 0;
}

.button () {
    border: 1px solid grey;
    cursor: pointer;
    padding: 5px;
    border-radius: 5px;
    user-select: none;
    -webkit-user-drag: none;
    display: inline-block;
    font-size: 1rem;
    line-height: 1rem;

    margin: 0 5px 5px 0;
    &:last-child {
        margin-right: 0;
    }

    &:hover {
        background-color: lightgreen;
    }

    &:active {
        position: relative;
        top: 1px;
    }
}

button {
    .button();
}

li {
    display: inline-block;
    margin: 0 10px;

    &.number {
        width: 100px;
        .button();
    }
}

a {
    color: #42b983;
}
</style>
