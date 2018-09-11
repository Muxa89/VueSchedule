<template>
    <thead>
        <tr>
            <sortable-table-header-cell
                rowspan="3"
                :list-of-states="{
                    'none': '',
                    'down': 'fas fa-sort-alpha-down',
                    'up': 'fas fa-sort-alpha-up'
                }"
            >
            ФИО
            </sortable-table-header-cell>
            <th v-for="(entry, index) in headerDates.years" :key="index" :colspan="entry.count">{{ entry.year }}</th>
        </tr>
        <tr>
            <th v-for="(entry, index) in headerDates.months" :key="index" :colspan="entry.count">{{ entry.month }}</th>
        </tr>
        <tr>
            <sortable-table-header-cell
                v-for="(date, index) in headerDates.days"
                :key="index"
                :list-of-states="{
                    'none': '',
                    'night': 'far fa-moon fa-xs',
                    'day': 'far fa-sun fa-xs'
                }"
                :name="date.fullDate.toISOString().slice(0, 10)"
                :start-state="sortOrder[date.fullDate.toISOString().slice(0, 10)]"
                @sort-order-changed="updateSortOrder($event)"
            > {{ date.day }}
            </sortable-table-header-cell>
        </tr>
    </thead>
</template>

<script>
import SortableTableHeaderCell from './SortableTableHeaderCell.vue'

export default {
    name: 'table-header',
    props: {
        datesProp: {
            type: Object,
            required: true
        }
    },
    components: {
        SortableTableHeaderCell
    },
    data: function () {
        return {
            dates: this.datesProp,
            sortOrder: (function (t) {
                var res = {}
                for (var i in t.datesProp) {
                    res[t.datesProp[i].toISOString().slice(0, 10)] = 'none'
                }
                return res
            })(this),
            monthNames: {
                0: 'Январь',
                1: 'Февраль',
                2: 'Март',
                3: 'Апрель',
                4: 'Май',
                5: 'Июнь',
                6: 'Июль',
                7: 'Август',
                8: 'Сентябрь',
                9: 'Октябрь',
                10: 'Ноябрь',
                11: 'Декабрь'
            }
        }
    },
    methods: {
        updateSortOrder: function (event) {
            for (var i in this.sortOrder) {
                if (event.name === i) {
                    this.$set(this.sortOrder, i, event.key)
                } else {
                    this.$set(this.sortOrder, i, 'none')
                }
            }
        }
    },
    computed: {
        headerDates: function () {
            var result = {
                years: [],
                months: [],
                days: []
            }

            for (var dateId in this.dates) {
                var date = this.dates[dateId]

                var year = result.years.find(function (value) { return value.year === date.getFullYear() })
                if (year === undefined) {
                    result.years.push({ year: date.getFullYear(), count: 1 })
                } else {
                    year.count++
                }

                var month = result.months.find(function (value) { return value.year === date.getFullYear() && value.month === date.getMonth() })
                if (month === undefined) {
                    result.months.push({ year: date.getFullYear(), month: date.getMonth(), count: 1 })
                } else {
                    month.count++
                }

                result.days.push({ day: date.getDate(), fullDate: date })
            }

            for (var i in result.months) {
                result.months[i].month = this.monthNames[result.months[i].month]
            }
            return result
        }
    }
}
</script>
