<template lang="pug">
.hello
    h1 {{ msg }}

    button.button(@click="plus1_each") +1 each
    button.button(@click="append") append 0
    ul
        li.number(
            v-for="(n, $index) in numbers"
            @click="increment_or_decrement_single($index)"
            :class="n.d > 0 ? 'plus' : 'minus'"
        ) {{ n.v }}
</template>

<script lang="ts">
import {defineComponent, toRefs, ref, Ref} from 'vue';

declare type RisingNumber = {
    v: number,  // value
    d: 1 | -1   // delta
}

export default defineComponent({
    props: {
        msg: {type: String, required: true}
    },
    setup(props: { msg: string }) {
        const {msg} = toRefs(props)

        const numbers: Ref<RisingNumber[]> = ref([
            {v: 3, d: 1},
            {v: 6, d: 1},
            {v: 9, d: 1}
        ] as RisingNumber[])

        const increment_or_decrement_single = (index: number): RisingNumber => {
            const target: RisingNumber = numbers.value[index]
            target.v = target.v + target.d
            if (target.v > 9) {
                target.d = -1
            } else if (target.v < 1) {
                target.d = 1
            }
            return target
        }

        const plus1_each = () => {
            numbers.value = numbers.value.map((n, index) => {
                n = increment_or_decrement_single(index)
                return n
            })
        }

        const append = () => {
            numbers.value = [...numbers.value, {v: 0, d: 1}]
            // numbers.value.push({v: 0. d: 1})
        }

        return {
            numbers,
            msg,
            append,
            increment_or_decrement_single,
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
