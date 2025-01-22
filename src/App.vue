<script setup lang="ts">
import { computed, ref, watch } from 'vue';

let vidaJogador = ref(100);
let vidaMonstro = ref(100);
const logsJogador = ref<string[]>([]);
const logsMonstro = ref<string[]>([]);

let jogo = ref(false);

let perdeu = ref(false);

let ganhou = ref(false);

let turnosDesdeEspecial = 3;

function numeroAleatorio(min: number, max: number) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

const adicionarLogJogador = (mensagem: string) => {
    logsJogador.value.unshift(mensagem); // Adiciona o log no início da lista
};

const adicionarLogMonstro = (mensagem: string) => {
    logsMonstro.value.unshift(mensagem); // Adiciona o log no início da lista
};

const jogoStart = () => {
    jogo.value = !jogo.value;
    vidaJogador.value = 100;
    vidaMonstro.value = 100;
    perdeu.value = false;
    ganhou.value = false;
    logsJogador.value = [];
    logsMonstro.value = [];
    turnosDesdeEspecial = 3;
}

const ataque = () => {
    const danoJogador = numeroAleatorio(5, 10); // Dano do jogador (entre 5 e 10)
    const danoMonstro = numeroAleatorio(danoJogador, danoJogador + 5); // Garantir que o dano do monstro seja >= do jogador

    vidaJogador.value -= danoMonstro;
    vidaMonstro.value -= danoJogador;

    console.log(`Dano do Jogador: ${danoJogador}`);
    console.log(`Dano do Monstro: ${danoMonstro}`);

    turnosDesdeEspecial++;

    adicionarLogJogador(`Jogador causou ${danoJogador} de dano ao monstro.`);
    adicionarLogMonstro(`Monstro causou ${danoMonstro} de dano ao jogador.`);
}

const ataqueEspecial = () => {

    if (turnosDesdeEspecial < 3) {
        alert('O ataque especial ainda não está disponivel');
        return;
    }

    const danoMonstro = numeroAleatorio(5, 10);
    const danoJogador = numeroAleatorio(danoMonstro, danoMonstro + 5);

    vidaJogador.value -= danoMonstro;
    vidaMonstro.value -= danoJogador;

    console.log('Ataque especial');
    console.log(`Dano do Jogador: ${danoJogador}`);
    console.log(`Dano do Monstro: ${danoMonstro}`);

    turnosDesdeEspecial = 0;

    adicionarLogJogador(`Jogador usou ataque especial e causou ${danoJogador} de dano ao monstro.`);
    adicionarLogMonstro(`Monstro causou ${danoMonstro} de dano ao jogador.`);
}

const curar = () => {
    const curaJogador = numeroAleatorio(6, 12); // Dano do jogador (entre 5 e 10)
    const danoMonstro = numeroAleatorio(5, 10); // Garantir que o dano do monstro seja >= do jogador

    vidaJogador.value -= danoMonstro;
    vidaJogador.value += curaJogador;

    console.log(`Cura jogador: ${curaJogador}`);
    console.log(`Dano do Monstro: ${danoMonstro}`);

    turnosDesdeEspecial++;

    adicionarLogJogador(`Jogador usou curar e recebeu ${curaJogador} de cura.`);
    adicionarLogMonstro(`Monstro causou ${danoMonstro} de dano ao jogador.`);
}

const logsIntercalados = computed(() => {
    const intercalados = [];
    const maxLength = Math.max(logsJogador.value.length, logsMonstro.value.length);

    for (let i = 0; i < maxLength; i++) {
        if (logsMonstro.value[i]) {
            intercalados.push({ tipo: 'monstro', mensagem: logsMonstro.value[i] });
        }
        if (logsJogador.value[i]) {
            intercalados.push({ tipo: 'jogador', mensagem: logsJogador.value[i] });
        }
    }

    return intercalados;
});


watch([vidaJogador, vidaMonstro], ([novoValorJogador, novoValorMonstro], []) => {
    if (novoValorMonstro <= 0) {
        jogo.value = false;
        ganhou.value = true;
    } else if (novoValorJogador <= 0) {
        jogo.value = false;
        perdeu.value = true;
    }
});

</script>


<template>
    <div class="m-10 p-10 shadow-lg flex justify-center space-x-7">
        <div id="jogador" class=" flex flex-col space-y-14 items-center w-1/2 justify-center">
            <h1 class="text-6xl">Jogador</h1>

            <div class="w-full flex flex-col items-center">
                <div class="h-8 w-9/12 border-2 border-gray-400">
                    <div :class="vidaJogador > 20 ? 'bg-lime-600' : 'bg-red-700'" class="h-full"
                        :style="{ width: vidaJogador + '%' }"></div>

                </div>
                <p class="text-3xl">{{ vidaJogador }}</p>
            </div>

        </div>
        <div id="jogador" class=" flex flex-col space-y-14 items-center w-1/2 justify-center">
            <h1 class="text-6xl">Monstro</h1>

            <div class="w-full flex flex-col items-center">
                <div class="h-8 w-9/12 border-2 border-gray-400">
                    <div :class="vidaMonstro > 20 ? 'bg-lime-600' : 'bg-red-700'" class="h-full"
                        :style="{ width: vidaMonstro + '%' }"></div>

                </div>
                <p class="text-3xl">{{ vidaMonstro }}</p>
            </div>

        </div>
    </div>

    <div v-show="perdeu" class="m-10 p-10 shadow-lg flex justify-center text-4xl">
        <div class="text-red-500 font-medium">Você perdeu! :(</div>
    </div>

    <div v-show="ganhou" class="m-10 p-10 shadow-lg flex justify-center text-4xl">
        <div class="font-medium text-lime-500">Você ganhou! :)</div>
    </div>

    <div class="m-10 p-10 shadow-lg flex justify-center text-4xl">
        <div v-if="!jogo" @click="jogoStart" class="bg-cyan-800 rounded-lg  p-4 text-white">Iniciar novo jogo</div>

        <div v-else-if="jogo" class="flex justify-center space-x-7">
            <div class="bg-red-600 hover:bg-red-700   p-4 rounded-lg text-white " @click="ataque">ATAQUE</div>
            <div class="bg-amber-400 hover:bg-amber-500 p-4 rounded-lg " @click="ataqueEspecial">ATAQUE ESPECIAL</div>
            <div class="bg-lime-400 hover:bg-lime-500 p-4 rounded-lg text-white" @click="curar">CURAR</div>
            <div class="bg-gray-500 hover:bg-gray-600 p-4 rounded-lg " @click="jogoStart">DESISTIR</div>
        </div>
    </div>

    <div v-show="logsIntercalados.length > 0" class="m-10 p-10 shadow-lg flex justify-center text-4xl items-center">
        <ul class="w-11/12">
            <li v-for="(log, index) in logsIntercalados" :key="index"
                :class="[log.tipo === 'jogador' ? 'bg-lime-500' : 'bg-red-500', 'p-4', 'm-4', 'text-white', 'w-full', 'rounded-lg', 'flex items-center flex-col']">
                {{ log.mensagem }}
            </li>
        </ul>
    </div>

</template>

<style scoped></style>
