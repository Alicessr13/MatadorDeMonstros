<script setup lang="ts">
import { ref, watch } from 'vue';

let vidaJogador = ref(100);
let vidaMonstro = ref(100);

let jogo = ref(false);

let perdeu = ref(false);

let ganhou = ref(false);

let turnosDesdeEspecial = 4;

function numeroAleatorio(min: number, max: number) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

const jogoStart = () => {
    jogo.value = !jogo.value;
    vidaJogador.value = 100;
    vidaMonstro.value = 100;
}

const ataque = () => {
    const danoJogador = numeroAleatorio(5, 10); // Dano do jogador (entre 5 e 10)
    const danoMonstro = numeroAleatorio(danoJogador, danoJogador + 5); // Garantir que o dano do monstro seja >= do jogador

    vidaJogador.value -= danoMonstro;
    vidaMonstro.value -= danoJogador;

    console.log(`Dano do Jogador: ${danoJogador}`);
    console.log(`Dano do Monstro: ${danoMonstro}`);

    turnosDesdeEspecial++;
}

const ataqueEspecial = () => {

    if (turnosDesdeEspecial < 4) {
        alert('O ataque especial ainda não está disponivel');
        return;
    }

    const danoMonstro = numeroAleatorio(5, 10);
    const danoJogador = numeroAleatorio(danoMonstro, danoMonstro + 5);

    vidaJogador.value -= danoMonstro;
    vidaMonstro.value -= danoJogador;

    console.log(`Dano do Jogador: ${danoJogador}`);
    console.log(`Dano do Monstro: ${danoMonstro}`);

    turnosDesdeEspecial = 0;
}


watch(vidaJogador, (novoValor, valorAntigo) => {
    if (novoValor <= 0) {
        jogo.value = true;
        perdeu.value = true;
    }
});

watch(vidaMonstro, (novoValor, valorAntigo) => {
    if (novoValor <= 0) {
        jogo.value = true;
        ganhou.value = true;
    }
});

</script>


<template>
    <div class="m-10 p-10 border-2 flex justify-center space-x-7">
        <div id="jogador" class=" flex flex-col space-y-14 items-center border-2 w-1/2 justify-center">
            <h1 class="text-6xl">Jogador</h1>

            <div class="w-full flex flex-col items-center">
                <div class="h-8 w-9/12 border-2 border-gray-400">
                    <div :class="vidaJogador > 20 ? 'bg-lime-600' : 'bg-red-700'" class="h-full"
                        :style="{ width: vidaJogador + '%' }"></div>

                </div>
                <p class="text-3xl">{{ vidaJogador }}</p>
            </div>

        </div>
        <div id="jogador" class=" flex flex-col space-y-14 items-center border-2 w-1/2 justify-center">
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

    <div v-show="perdeu" class="m-10 p-10 border-2 flex justify-center text-4xl">
        <div>Você perdeu! :(</div>
    </div>

    <div v-show="ganhou" class="m-10 p-10 border-2 flex justify-center text-4xl">
        <div>Você ganhou! :)</div>
    </div>

    <div class="m-10 p-10 border-2 flex justify-center text-4xl">
        <div v-if="!jogo" @click="jogoStart">Iniciar novo jogo</div>

        <div v-else-if="jogo" class="flex justify-center space-x-7">
            <div class="bg-red-600 hover:bg-red-700   p-4 rounded-lg text-white " @click="ataque">ATAQUE</div>
            <div class="bg-amber-400 hover:bg-amber-500 p-4 rounded-lg " @click="ataqueEspecial">ATAQUE ESPECIAL</div>
            <div class="bg-lime-400 hover:bg-lime-500 p-4 rounded-lg text-white">CURAR</div>
            <div class="bg-gray-500 hover:bg-gray-600 p-4 rounded-lg " @click="jogoStart">DESISTIR</div>
        </div>
    </div>


</template>

<style scoped></style>
