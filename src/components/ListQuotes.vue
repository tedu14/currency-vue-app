<template>
    <table class="table">
        <thead>
            <tr>
                <th>Código</th>
                <th>Nome</th>
                <th>Máximo</th>
                <th>Mínimo</th>
                <th>Variação</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(quote, key) in quotes" :key="key">
                <td>{{ key }}</td>
                <td>{{ quote.name }}</td>
                <td>{{ quote.high }}</td>
                <td>{{ quote.low }}</td>
                <td>
                    <span 
                        class="label label-rounded text-small"
                        :class="{
                            'label-error': quote.pctChange < 0,
                            'label-success': quote.pctChange > 0
                        }"
                    >
                        {{ quote.pctChange }}
                    </span>
                </td>
                <td>
                    <a 
                        v-if="!listenQuotes.includes(key)"
                        class="btn btn-primary btn-sm tooltip tooltip-left"
                        rel="noopener noreferrer"
                        data-tooltip="Seguir"
                        @click.prevent="sendListenQuote(key)"
                    >
                        <i class="icon icon-plus"></i>
                    </a>
                    <a 
                        v-else
                        href="#" 
                        class="btn btn-error btn-sm tooltip tooltip-left"
                        data-tooltip="Remover"
                        @click.prevent="sendUnlistenQuote(key)"
                        >
                        <i class="icon icon-minus"></i>
                    </a>
                </td>
            </tr>
        </tbody>
    </table>
</template>

<script>
export default {
    name: 'ListQuotes',
    props: {
        quotes: {
            type: Object,
            required: true
        },
        listenQuotes: {
            type: Array,
            required: true
        }
    },
    methods: {
        sendListenQuote(key){
            this.$emit('listen', key)
        },
        sendUnlistenQuote(key){
            this.$emit('unlisten', key)
        }
    },
    emits: ["listen", 'unlisten']
}
</script>