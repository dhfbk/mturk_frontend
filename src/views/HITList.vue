<template>
    <div class="relative lg:w-5/6 px-8 pt-6 pb-8 mb-4 flex flex-col my-4 mx-auto">
        <h1 class="text-2xl mb-4 text-orange-400">Hit del progetto "nomeProgetto"</h1>
        <div class="container mx-auto">
            <div class="flex flex-row justify-end mb-2">
                <p class="text-md my-auto mr-1">Stato:</p>
                <div class="relative">
                    <select
                        class="appearance-none h-full rounded border block appearance-none w-full bg-white border-gray-400 text-gray-700 py-2 px-4 pr-8 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
                        name="sortBy"
                        id="sortBy"
                        @change="sortBy(sortType)"
                        v-model="sortType"
                    >
                        <option
                            v-for="item in sortOptions"
                            :key="item.value"
                            :value="item.value"
                        >{{item.text}}</option>
                    </select>
                    <div
                        class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700"
                    >
                        <svg
                            class="fill-current h-4 w-4"
                            xmlns="http://www.w3.org/2000/svg"
                            viewBox="0 0 20 20"
                        >
                            <path
                                d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"
                            />
                        </svg>
                    </div>
                </div>
            </div>
            <table class="w-full shadow-lg rounded">
                <thead>
                    <tr class="text-center bg-orange-300 border-b border-grey uppercase">
                        <th class="py-2 px-2 text-sm text-gray-700">HIT ID</th>
                        <th class="py-2 px-2 text-sm text-gray-700">Stato</th>
                        <th class="py-2 px-2 hidden sm:table-cell text-sm text-gray-700">Creazione</th>
                        <th
                            class="py-2 px-2 hidden md:table-cell text-sm text-gray-700"
                        >Tot. assignment</th>
                        <th class="py-2 px-2 hidden md:table-cell text-sm text-gray-700">Completati</th>
                        <th class="py-2 px-2 hidden lg:table-cell text-sm text-gray-700">In sospeso</th>
                        <th class="py-2 px-2 hidden lg:table-cell text-sm text-gray-700">Disponibili</th>
                    </tr>
                </thead>
                <tbody class="bg-white text-center">
                    <tr
                        class="border-b border-grey-light hover:bg-gray-100"
                        v-for="hit in hitDaMostrare"
                        :key="hit.HITId"
                    >
                        <td class="table-cell py-2">
                            <p class="text-sm text-gray-700 font-medium">{{hit.HITId}}</p>
                        </td>
                        <td
                            class="table-cell py-2 whitespace-no-wrap text-blue-900 text-sm leading-5"
                        >
                            <span class="relative inline-block px-3 py-1 italic text-green-900">
                                <span
                                    aria-hidden
                                    v-if="hit.HITStatus == 'Reviewable'"
                                    class="absolute inset-0 bg-green-200 opacity-50 rounded-full"
                                ></span>
                                <span
                                    aria-hidden
                                    v-if="hit.HITStatus == 'Assignable'"
                                    class="absolute inset-0 bg-yellow-400 opacity-50 rounded-full"
                                ></span>
                                <span
                                    aria-hidden
                                    v-if="hit.HITStatus == 'Disposed'"
                                    class="absolute inset-0 bg-red-400 opacity-50 rounded-full"
                                ></span>
                                <span class="relative text-xs">{{hit.HITStatus}}</span>
                            </span>
                        </td>
                        <td class="hidden sm:table-cell py-2">
                            <p class="text-sm text-gray-700 font-medium">{{hit.CreationTime}}</p>
                        </td>
                        <td class="hidden md:table-cell py-2">
                            <p class="text-sm text-gray-700 font-medium">{{hit.MaxAssignments}}</p>
                        </td>
                        <td class="hidden md:table-cell py-2">
                            <p
                                class="text-sm text-gray-700 font-medium"
                            >{{hit.NumberOfAssignmentsCompleted}}</p>
                        </td>
                        <td class="hidden lg:table-cell py-2">
                            <p
                                class="text-sm text-gray-700 font-medium"
                            >{{hit.NumberOfAssignmentsPending}}</p>
                        </td>
                        <td class="hidden lg:table-cell py-2">
                            <p
                                class="text-sm text-gray-700 font-medium"
                            >{{hit.NumberOfAssignmentsAvailable}}</p>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
<script>
export default {
    name: 'HITList',
    data() {
        return {
            sortType: 'all',
            sortOptions: [
                { text: 'Tutte', value: 'all' },
                { text: 'Assignable', value: 'Assignable' },
                { text: 'Disposed', value: 'Disposed' },
                { text: 'Reviewable', value: 'Reviewable' },
            ],
            filtri: {
                stato: 'all',
                creazione: 'all',
            },
            datiHIT: [
                {
                    HITId: '76f8gf7g68cd',
                    HITTypeId: 'string',
                    HITGroupId: 'string',
                    HITLayoutId: 'string',
                    CreationTime: '2015-01-01',
                    Title: 'string',
                    Description: 'string',
                    Question: 'string',
                    Keywords: 'string',
                    HITStatus: 'Assignable',
                    MaxAssignments: 123,
                    Reward: 'string',
                    AutoApprovalDelayInSeconds: 123,
                    Expiration: '2015-01-01',
                    AssignmentDurationInSeconds: 123,
                    HITReviewStatus: 'NotReviewed',
                    NumberOfAssignmentsPending: 123,
                    NumberOfAssignmentsAvailable: 123,
                    NumberOfAssignmentsCompleted: 123,
                    selected: false,
                },
                {
                    HITId: 'stri454g5g5kghng',
                    HITTypeId: 'string',
                    HITGroupId: 'string',
                    HITLayoutId: 'string',
                    CreationTime: '2015-01-01',
                    Title: 'string',
                    Description: 'string',
                    Question: 'string',
                    Keywords: 'string',
                    HITStatus: 'Reviewable',
                    MaxAssignments: 123,
                    Reward: 'string',
                    AutoApprovalDelayInSeconds: 123,
                    Expiration: '2015-01-01',
                    AssignmentDurationInSeconds: 123,
                    HITReviewStatus: 'NotReviewed',
                    NumberOfAssignmentsPending: 123,
                    NumberOfAssignmentsAvailable: 123,
                    NumberOfAssignmentsCompleted: 123,
                    selected: false,
                },
                {
                    HITId: '3j747587d4k8',
                    HITTypeId: 'string',
                    HITGroupId: 'string',
                    HITLayoutId: 'string',
                    CreationTime: '2015-01-01',
                    Title: 'string',
                    Description: 'string',
                    Question: 'string',
                    Keywords: 'string',
                    HITStatus: 'Disposed',
                    MaxAssignments: 123,
                    Reward: 'string',
                    AutoApprovalDelayInSeconds: 123,
                    Expiration: '2015-01-01',
                    AssignmentDurationInSeconds: 123,
                    HITReviewStatus: 'ReviewedInappropriate',
                    NumberOfAssignmentsPending: 123,
                    NumberOfAssignmentsAvailable: 123,
                    NumberOfAssignmentsCompleted: 123,
                    selected: false,
                },
            ],
            hitDaMostrare: [],
        }
    },
    created() {
        this.filtraHIT()
    },
    methods: {
        sortBy(mode) {
            if (this.filtri.stato != mode) {
                this.filtri.stato = mode
                this.filtraHIT()
            }
        },
        filtraHIT() {
            if (this.filtri.stato == 'all') {
                this.hitDaMostrare = JSON.parse(JSON.stringify(this.datiHIT))
                console.log('BIG')
            } else {
                this.hitDaMostrare = JSON.parse(JSON.stringify(this.datiHIT))
                let i = 0
                while (this.hitDaMostrare[i] != undefined) {
                    if (this.hitDaMostrare[i].HITStatus != this.filtri.stato) {
                        this.hitDaMostrare.splice(i, 1)
                        i--
                    }
                    i++
                }
            }
        },
    },
}
</script>

<style scoped>
</style>