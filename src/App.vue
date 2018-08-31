<template>
    <div id="app">
        <schedule-table
            :departmentProp="department2"
            @duplicate-dates="onDuplicateDates">
        </schedule-table>
    </div>
</template>

<script>
import ScheduleTable from './components/ScheduleTable'

export default {
    name: 'App',
    components: {
        ScheduleTable
    },
    methods: {
        onDuplicateDates: function (event) {
            console.log(event)
        }
    },
    data () {
        return {
            department: [
                {
                    id: 1,
                    name: 'Ivanov',
                    schedule: [
                        {
                            id: 0,
                            date: new Date(2018, 1, 1),
                            shiftLength: 0
                        }, {
                            id: 1,
                            date: new Date(2018, 1, 2),
                            shiftLength: 1
                        }, {
                            id: 2,
                            date: new Date(2018, 1, 3),
                            shiftLength: 2
                        }, {
                            id: 3,
                            date: new Date(2018, 1, 4),
                            shiftLength: 4
                        }, {
                            id: 4,
                            date: new Date(2018, 1, 5),
                            shiftLength: 5
                        }
                    ]
                }, {
                    id: 2,
                    name: 'Petrov',
                    schedule: [
                        {
                            id: 5,
                            date: new Date(2018, 1, 1),
                            shiftLength: 10
                        }, {
                            id: 6,
                            date: new Date(2018, 1, 2),
                            shiftLength: 11
                        }, {
                            id: 7,
                            date: new Date(2018, 1, 3),
                            shiftLength: 12
                        }, {
                            id: 8,
                            date: new Date(2018, 1, 4),
                            shiftLength: 14
                        }, {
                            id: 9,
                            date: new Date(2018, 1, 5),
                            shiftLength: 15
                        }
                    ]
                }, {
                    id: 3,
                    name: 'Sidorov',
                    schedule: [
                        {
                            id: 10,
                            date: new Date(2018, 0, 1),
                            shiftLength: 2
                        }, {
                            id: 10,
                            date: new Date(2018, 1, 1),
                            shiftLength: 20
                        }, {
                            id: 11,
                            date: new Date(2018, 1, 2),
                            shiftLength: 21
                        }, {
                            id: 12,
                            date: new Date(2018, 1, 3),
                            shiftLength: 22
                        }, {
                            id: 13,
                            date: new Date(2018, 1, 4),
                            shiftLength: 24
                        }, {
                            id: 14,
                            date: new Date(2018, 1, 5),
                            shiftLength: 25
                        }, {
                            id: 16,
                            date: new Date(2018, 1, 10),
                            shiftLength: 25
                        }, {
                            id: 17,
                            date: new Date(2018, 1, 10),
                            shiftLength: 25
                        }
                    ]
                }
            ],
            department2: {
                workers: {
                    1: {name: 'Ivanov'},
                    2: {name: 'Petrov'},
                    3: {name: 'Sidorov'}
                },
                dates: {
                    1: new Date(2018, 0, 1),
                    2: new Date(2018, 0, 2),
                    3: new Date(2018, 0, 3),
                    4: new Date(2018, 0, 4),
                    5: new Date(2018, 0, 5)
                },
                schedule: {
                    1: { 1: 0, 2: 1, 3: 2, 4: 3, 5: 4 },
                    2: { 1: 5, 2: 6, 3: 7, 4: 8, 5: 9 },
                    3: { 1: 10, 2: 11, 3: 12, 4: 13, 5: 14 }
                }
            }
        }
    }
}
</script>
