<template>
    <div class="relative lg:w-5/6 px-8 pt-6 pb-8 mb-4 flex flex-col my-4 mx-auto">
        <modalEliminazione v-if="modal" @hideModal="toggleModal" />
        <div class="flex justify-between flex-wrap">
            <h1 class="text-2xl mb-4 text-orange-400">{{datiProgetto.nome}}</h1>
            <div class="flex relative">
                <button
                    v-show="!mini"
                    type="submit"
                    class="py-2 px-4 bg-transparent rounded-md transition duration-150 ease-in-out border-2 border-solid border-green-400 hover:bg-green-400 mr-2 focus:outline-none"
                >Pubblica</button>
                <button
                    v-show="!mini"
                    type="submit"
                    class="py-2 px-4 bg-transparent rounded-md transition duration-150 ease-in-out border-2 border-solid border-orange-400 hover:bg-orange-400 mr-2 focus:outline-none"
                >Risultati</button>
                <button
                    @click="dropdownOpen = !dropdownOpen"
                    v-click-outside="hide"
                    class="py-2 px-2 bg-transparent rounded-md transition duration-150 ease-in-out border-2 border-solid border-orange-400 hover:bg-orange-400 focus:outline-none"
                >
                    <svg style="width:24px;height:24px" viewBox="0 0 24 24" v-if="!dropdownOpen">
                        <path
                            fill="currentColor"
                            d="M7.41,8.58L12,13.17L16.59,8.58L18,10L12,16L6,10L7.41,8.58Z"
                        />
                    </svg>
                    <svg style="width:24px;height:24px" viewBox="0 0 24 24" v-else>
                        <path
                            fill="currentColor"
                            d="M7.41,15.41L12,10.83L16.59,15.41L18,14L12,8L6,14L7.41,15.41Z"
                        />
                    </svg>
                </button>
                <transition name="fade">
                    <div
                        v-show="dropdownOpen"
                        class="absolute bottom-1 right-0 mt-16 w-56 bg-white rounded-md shadow-xl z-20"
                    >
                        <a
                            v-show="mini"
                            class="block px-4 py-2 text-sm capitalize text-gray-700 transition duration-150 ease-in-out hover:bg-orange-400 rounded-t-md"
                        >Pubblica</a>
                        <a
                            v-show="mini"
                            class="block px-4 py-2 text-sm capitalize text-gray-700 transition duration-150 ease-in-out hover:bg-orange-400"
                        >Risultati</a>
                        <a
                            class="block px-4 py-2 text-sm capitalize text-gray-700 transition duration-150 ease-in-out hover:bg-orange-400 rounded-t-md"
                            @click="toggleModal()"
                        >Elimina</a>
                        <a
                            class="block px-4 py-2 text-sm capitalize text-gray-700 transition duration-150 ease-in-out hover:bg-orange-400"
                        >Modifica</a>
                        <a
                            class="block px-4 py-2 text-sm capitalize text-gray-700 transition duration-150 ease-in-out hover:bg-orange-400"
                        >Carica dati</a>
                        <a
                            class="block px-4 py-2 text-sm capitalize text-gray-700 transition duration-150 ease-in-out hover:bg-orange-400 rounded-b-md"
                        >Carica gold</a>
                    </div>
                </transition>
            </div>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2">
            <div class="mx-0 sm:mx-2">
                <cardInfo :titoli="titoliCard.titoli1" :dati="datiCard.dati1" />
                <cardInfo :titoli="titoliCard.titoli2" :dati="datiCard.dati2" />
                <cardInfo :titoli="titoliCard.titoli3" :dati="datiCard.dati3" />
            </div>
            <div class="mx-0 sm:mx-2">
                <cardAnalytics :dati="datiCardAnalytics.cardHIT" />
                <cardAnalytics :dati="datiCardAnalytics.cardAggregate" />
            </div>
        </div>
    </div>
</template>

<script>
import ClickOutside from 'vue-click-outside'
import modalEliminazione from '../components/modalEliminazione.vue'
import cardInfo from '../components/cardInfo.vue'
import cardAnalytics from '../components/cardAnalyticsVisualizzaProgetto.vue'

export default {
    name: 'visualizzaProgetto',
    directives: {
        ClickOutside,
    },
    components: {
        modalEliminazione,
        cardInfo,
        cardAnalytics,
    },
    data() {
        return {
            datiProgetto: {
                nome: 'nomeProgetto',
                titolo: "Italy's food",
                descrizione:
                    "If you know Italian food, we ask you to take a look at various different serving pictures and tell us if you think it's italian or not.",
                keywords: 'food, italy, image, recognition, culture',
                stato: 'In corso',
                ricompensa: '0.25',
                creazione: '2020-09-02',
                numLavoratori: 5,
                tempoMax: 60,
                scadenza: '2020-09-10',
                autoApproval: 86400,
                layoutID: 'hdf7777cv4cc34c',
                parametri: 3,
                totaleHIT: 400,
                HITcompletate: 200,
                HITdisponibili: 90,
                HITinCorso: 110,
                risposteSI: 180,
                risposteNO: 20,
                completateProgress: 0,
                disponibiliProgress: 0,
                incorsoProgress: 0,
                siProgress: 0,
                noProgress: 0,
            },
            titoliCard: {
                titoli1: [],
                titoli2: [],
                titoli3: [],
            },
            datiCard: {
                dati1: [],
                dati2: [],
                dati3: [],
            },
            datiCardAnalytics: {},
            dropdownOpen: false,
            mini: false,
            modal: false,
        }
    },
    created() {
        this.elaboraTempo(this.datiProgetto.tempoMax)
        this.elaboraTempo(this.datiProgetto.autoApproval)
        this.calcolaProgress()
        this.impostaDatiCard()
    },
    mounted() {
        this.popupItem = this.$el
    },
    methods: {
        impostaDatiCard() {
            this.titoliCard.titoli1 = [
                'Titolo',
                'Descrizione',
                'Keywords',
                'Data creazione',
                'Stato',
            ]
            this.datiCard.dati1 = [
                this.datiProgetto.titolo,
                this.datiProgetto.descrizione,
                this.datiProgetto.keywords,
                this.datiProgetto.creazione,
                this.datiProgetto.stato,
            ]
            this.titoliCard.titoli2 = [
                'Ricompensa per ogni assignment',
                'Numero di lavoratori/assignment per task',
                'Tempo massimo',
                'Scadenza',
                'Auto approva e paga lavoratori in',
            ]
            this.datiCard.dati2 = [
                this.datiProgetto.ricompensa,
                this.datiProgetto.numLavoratori,
                this.datiProgetto.tempoMax,
                this.datiProgetto.scadenza,
                this.datiProgetto.autoApproval,
            ]
            this.titoliCard.titoli3 = ['Layout ID', 'Numero parametri']
            this.datiCard.dati3 = [this.datiProgetto.layoutID, this.datiProgetto.parametri]
            this.datiCardAnalytics = {
                cardHIT: {
                    titolo: 'Totale HIT',
                    totale: this.datiProgetto.totaleHIT,
                    type: 'HIT',
                    ellipse_progress: {
                        progress1: {
                            progress: this.datiProgetto.completateProgress,
                            legend_value: this.datiProgetto.HITcompletate,
                            color: '#f6ad55',
                            half: true,
                            angle: 0,
                            caption: 'Completate',
                        },
                        progress2: {
                            progress: this.datiProgetto.disponibiliProgress,
                            legend_value: this.datiProgetto.HITdisponibili,
                            color: '#f6ad55',
                            half: true,
                            angle: 0,
                            caption: 'Disponibili',
                        },
                        progress3: {
                            progress: this.datiProgetto.incorsoProgress,
                            legend_value: this.datiProgetto.HITinCorso,
                            color: '#f6ad55',
                            half: true,
                            angle: 0,
                            caption: 'In corso',
                        },
                    },
                },
                cardAggregate: {
                    titolo: 'Risposte aggregate',
                    totale: this.datiProgetto.HITcompletate,
                    type: 'aggregate',
                    ellipse_progress: {
                        progress1: {
                            progress: this.datiProgetto.siProgress,
                            legend_value: this.datiProgetto.risposteSI,
                            color: '#f6ad55',
                            half: false,
                            angle: 0,
                            caption: 'SI',
                        },
                        progress2: {
                            progress: this.datiProgetto.noProgress,
                            legend_value: this.datiProgetto.risposteNO,
                            color: '#f6ad55',
                            half: false,
                            angle: 0,
                            caption: 'NO',
                        },
                    },
                },
            }
        },
        toggleModal() {
            this.modal = !this.modal
            this.hide()
        },
        hide() {
            this.dropdownOpen = false
        },
        calcolaProgress() {
            this.datiProgetto.completateProgress =
                (100 * this.datiProgetto.HITcompletate) / this.datiProgetto.totaleHIT
            this.datiProgetto.disponibiliProgress =
                (100 * this.datiProgetto.HITdisponibili) / this.datiProgetto.totaleHIT
            this.datiProgetto.incorsoProgress =
                (100 * this.datiProgetto.HITinCorso) / this.datiProgetto.totaleHIT
            this.datiProgetto.siProgress =
                (100 * this.datiProgetto.risposteSI) / this.datiProgetto.HITcompletate
            this.datiProgetto.noProgress =
                (100 * this.datiProgetto.risposteNO) / this.datiProgetto.HITcompletate
        },
        elaboraTempo(tempo) {
            var tmp = 0
            if (tempo < 60) {
                tmp = Math.floor(tempo) + ' secondi'
            } else if (tempo < 3600) {
                tmp = Math.floor(tempo / 60) + ' minuti'
            } else if (tempo < 86400) {
                tmp = Math.floor(tempo / 3600) + ' ore'
            } else {
                tmp = Math.floor(tempo / 86400) + ' giorni'
            }
            if (tempo == this.datiProgetto.tempoMax) {
                this.datiProgetto.tempoMax = tmp
            } else {
                this.datiProgetto.autoApproval = tmp
            }
        },
    },
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: all 0.25s;
}
.fade-enter,
.fade-leave-to {
    opacity: 0;
}
</style>