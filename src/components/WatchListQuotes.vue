<template>
    <list-quotes 
        :quotes="quotes" 
        :listenQuotes="listenQuotes"
        @unlisten="onUnlisten"
    >
    </list-quotes>
    <div class="mt-2 text-right">
        <cite class="text-small">
            Atualizará em: <b>{{ nextUpdateTime }} segundos</b>
        </cite>
    </div>
</template>

<script>
import {  onMounted, onUnmounted, reactive, ref, toRefs, watch } from 'vue'
import ListQuotes from './ListQuotes.vue'
import api from '../services/api'

export default {
    components: { ListQuotes },
    name: 'WatchListQuotes',
    props: {
        listenQuotes: {
            type: Array,
            required: true
        }
    },
    setup(props, { emit }){
        const interval = ref(null)
        const quotes = ref({})
        const currentUpdateTime = 60;
        const nextUpdateTime = ref(currentUpdateTime)

        const methods = reactive({
            async reflesh(){
                const {data} = await api.listen(props.listenQuotes)

                quotes.value = data
            },
            onUnlisten(code){
                emit('unlisten', code)
            },

            reatartInterval(){
                clearInterval(interval.value)

                nextUpdateTime.value = currentUpdateTime;

                interval.value = setInterval(() => {
                    nextUpdateTime.value -= 1

                    if(nextUpdateTime.value === 0){
                        nextUpdateTime.value = currentUpdateTime
                        this.reflesh()
                    }
                }, 1000)
            }
        })

        const repeatMethodsWhenMounted = () => {
            methods.reflesh()
            methods.reatartInterval()
        }

        onMounted(repeatMethodsWhenMounted)

        watch(props, repeatMethodsWhenMounted)

        onUnmounted(() => clearInterval(interval.value))

        return {
            ...toRefs(methods),
            quotes,
            nextUpdateTime            
            }
    },
    emits: ['unlisten']
}
</script>