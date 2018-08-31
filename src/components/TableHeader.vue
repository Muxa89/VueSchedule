<template>
    <thead>
        <tr>
            <th rowspan="3">ФИО</th>
            <th v-for="(entry, index) in headerDates.years" :key="index" :colspan="entry.count">{{ entry.year }}</th>
        </tr>
        <tr>
            <th v-for="(entry, index) in headerDates.months" :key="index" :colspan="entry.count">{{ entry.month }}</th>
        </tr>
        <tr>
            <th v-for="(day, index) in headerDates.days" :key="index">{{ day }}</th>
        </tr>
    </thead>
</template>

<script>
export default {
    name: 'table-header',
    props: {
        datesProp: {
            type: Object,
            required: true
        }
    },
    data: function () {
        return {
            dates: this.datesProp
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

                result.days.push(date.getDate())
            }

            for (var m in result.months) {
                if (result.months[m].month === 0) {
                    result.months[m].month = 'Январь'
                } else if (result.months[m].month === 1) {
                    result.months[m].month = 'Февраль'
                } else if (result.months[m].month === 2) {
                    result.months[m].month = 'Март'
                } else if (result.months[m].month === 3) {
                    result.months[m].month = 'Апрель'
                } else if (result.months[m].month === 4) {
                    result.months[m].month = 'Май'
                } else if (result.months[m].month === 5) {
                    result.months[m].month = 'Июнь'
                } else if (result.months[m].month === 6) {
                    result.months[m].month = 'Июль'
                } else if (result.months[m].month === 7) {
                    result.months[m].month = 'Август'
                } else if (result.months[m].month === 8) {
                    result.months[m].month = 'Сентябрь'
                } else if (result.months[m].month === 9) {
                    result.months[m].month = 'Октябрь'
                } else if (result.months[m].month === 10) {
                    result.months[m].month = 'Ноябрь'
                } else if (result.months[m].month === 11) {
                    result.months[m].month = 'Декабрь'
                }
            }
            return result
        }
    }
}
</script>
