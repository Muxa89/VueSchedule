<template>
    <th
            style="
                position: relative;
                -webkit-user-select: none;  /* Chrome all / Safari all */
                -moz-user-select: none;     /* Firefox all */
                -ms-user-select: none;      /* IE 10+ */
                user-select: none;          /* Likely future */
            "
            @click.stop.prevent="nextState">

        <div>
            <slot></slot>
        </div>

        <span
            :class="this.listOfStates[currentKey]"
            style="
                position: absolute;
                bottom:2px;
                right: 2px
            "
        />
    </th>
</template>

<script>
export default {
    name: 'sortable-table-header-cell',
    props: {
        listOfStates: Object,
        startState: String,
        name: String
    },
    data: function () {
        return {
            currentKey: this.startState === undefined ? Object.keys(this.listOfStates)[0] : this.startState
        }
    },
    watch: {
        startState: function (value) {
            this.currentKey = value === undefined ? Object.keys(this.listOfStates)[0] : value
        }
    },
    methods: {
        nextState: function () {
            var keys = Object.keys(this.listOfStates)
            var currentIndex = keys.indexOf(this.currentKey)
            currentIndex = (currentIndex + 1) % keys.length
            this.currentKey = keys[currentIndex]
            this.$emit('sort-order-changed', {
                name: this.name,
                key: this.currentKey,
                value: this.listOfStates[this.currentKey]
            })
        }
    }
}
</script>
