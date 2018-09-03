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
            :class="this.listOfStates[current]"
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
            current: this.startState === undefined ? Object.keys(this.listOfStates)[0] : this.startState
        }
    },
    methods: {
        nextState: function () {
            var currentIndex = Object.keys(this.listOfStates).indexOf(this.current)
            currentIndex++
            currentIndex %= Object.keys(this.listOfStates).length
            this.current = Object.keys(this.listOfStates)[currentIndex]
        }
    }
}
</script>
