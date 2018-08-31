<template>
    <div>
        <div v-if="!datesValidator.isValid || !scheduleValidator.isValid">
            Обнаружены ошибки во входных данных!
            <ul>
                <li v-for="(error, index) in datesValidator.errors" :key="index">
                    {{ error }}
                </li>
                <li v-for="(error, index) in scheduleValidator.errors" :key="index">
                    {{ error }}
                </li>
            </ul>
        </div>
        <table v-if="datesValidator.isValid && scheduleValidator.isValid" class="table table-bordered">
            <table-header
                :datesProp="this.dates">
            </table-header>
            <tbody>
                <tr v-for="(worker, workerId) in workers" :key="workerId">
                    <th>{{ worker.name }}</th>
                    <td v-for="(date, dateId) in dates" :key="dateId">
                        <input v-model="schedule[workerId][dateId]">
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import TableHeader from './TableHeader.vue'

export default {
    name: 'schedule-table',
    props: {
        departmentProp: {
            type: Object,
            required: true
        }
    },
    components: {
        TableHeader
    },
    methods: {
        convertDate: function (date) {
            return date.toISOString().slice(0, 10)
        },
        getUniqueDates: function (dates) {
            var result = {}

            for (var i in dates) {
                result[dates[i].toISOString().slice(0, 10)] = dates[i]
            }

            return Object.values(result)
        },
        checkIfDatesIsUnique: function (dates) {
            var result = {
                isValid: true,
                errors: []
            }

            var datesCount = {}
            for (var i in dates) {
                var d = dates[i].toISOString().slice(0, 10)
                datesCount[d] = d in datesCount ? datesCount[d] + 1 : 1
            }

            for (var j in datesCount) {
                if (datesCount[j] > 1) {
                    result.isValid = false
                    result.errors.push('Найдена неуникальная дата: ' + j)
                }
            }

            return result
        },
        checkIfScheduleIsConsistent: function (dates, workers, schedule) {
            var result = {
                isValid: true,
                errors: []
            }
            var dateId, workerId

            // Проверяем, что для всех записей в графике есть соответствующие сущности работников и дат
            for (workerId in schedule) {
                if (!(workerId in workers)) {
                    result.isValid = false
                    result.errors.push('В графике найден идентификатор сотрудника, которого нет в общем списке сотрудников: ' + workerId)
                }
                for (dateId in schedule[workerId]) {
                    if (!(dateId in dates)) {
                        result.isValid = false
                        result.errors.push('В графике найден идентификатор даты, которой нет в общем списке дат: ' + dateId)
                    }
                }
            }

            if (!result.isValid) {
                return result
            }

            // Проверяем что для всех работников есть информация по всем указанным датам и она только одна
            for (workerId in workers) {
                var datesCount = {}
                for (dateId in dates) {
                    datesCount[dateId] = 0
                }
                for (dateId in schedule[workerId]) {
                    datesCount[dateId] += 1
                }
                for (dateId in dates) {
                    if (datesCount[dateId] === 0) {
                        result.isValid = false
                        result.errors.push(
                            'Для сотрудника с идентификтором ' +
                            workerId + ' (' + workers[workerId].name + ') ' +
                            'найдена дата, для которой отсутствует продолжительность смены: ' +
                            this.convertDate(dates[dateId]) + '.'
                        )
                    }
                }
            }

            return result
        }
    },
    data: function () {
        return {
            workers: this.departmentProp.workers,
            dates: this.departmentProp.dates,
            schedule: this.departmentProp.schedule
        }
    },
    computed: {
        datesValidator: function () {
            return this.checkIfDatesIsUnique(this.dates)
        },
        scheduleValidator: function () {
            return this.checkIfScheduleIsConsistent(this.dates, this.workers, this.schedule)
        }
    }
}
</script>

<style>
    .table {
        width: auto;
    }
    .table thead th {
        vertical-align: middle;
        text-align: center;
        word-break: break-all;
        padding: 0.75rem 0px 0.75rem 0px;
    }
    .table tbody th {
        vertical-align: middle;
        text-align: center;
    }
    .table td {
        width: 30px;
        max-width: 30px;
        padding: 0px;
    }
    .table input {
        padding-top: 0.75rem;
        padding-bottom: 0.75rem;
        border-color: transparent;
        text-align: center;
        width: 100%;
    }
</style>
