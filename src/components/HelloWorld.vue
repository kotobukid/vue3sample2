<template lang="pug">
.hello
    h1 {{ msg }}

    button.button(@click="plus1_each") +1 each
    button.button(@click="append") append 0
    ul
        li.number(
            v-for="(n, $index) in numbers"
            @click="increment_single($index)"
            :class="n[1] > 0 ? 'plus' : 'minus'"
        ) {{ n[0] }}
</template>

<script lang="ts">
import {defineComponent, toRefs, ref, Ref} from 'vue';

export default defineComponent({
    props: {
        msg: {type: String, required: true}
    },
    setup(props: { msg: string }) {
        const {msg} = toRefs(props)

        const numbers: Ref<[number, number][]> = ref([
            [0, 1],
            [0, 1],
            [0, 1]
        ] as [number, number][])

        const increment = (index: number) => {
            numbers.value[index] = increment_single(index)
        }

        const increment_single = (index: number): [number, number] => {
            const target: [number, number] = numbers.value[index]
            target[0] = target[0] + target[1]
            if (target[0] > 9) {
                target[1] = -1
            } else if (target[0] < 1) {
                target[1] = 1
            }
            return target
        }

        const plus1_each = () => {
            numbers.value = numbers.value.map((n, index) => {
                n = increment_single(index)
                return n
            })
        }

        const append = () => {
            numbers.value = [...numbers.value, [0, 1]]
            // numbers.value.push([0, 1])
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

        &.plus {
            background-color: pink;
        }

        &.minus {
            background-color: lightblue;
        }
    }
}

a {
    color: #42b983;
}
</style>
