<script setup lang="ts">
import { ref } from 'vue';

const vidaJogador = ref(100);
const vidaMonstro = ref(80);

const jogo = ref(false);

const jogoStart = () => {
    jogo.value = !jogo.value;
    vidaJogador.value = 100;
    vidaMonstro.value = 100;
}

const ataque = () => {
    vidaJogador.value -= 10;
    vidaMonstro.value -= 10;
}

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

    <div class="m-10 p-10 border-2 flex justify-center text-4xl">
        <div v-if="!jogo" @click="jogoStart">Iniciar novo jogo</div>

        <div v-else-if="jogo" class="flex justify-center space-x-7">
            <div class="bg-red-600 hover:bg-red-700   p-4 rounded-lg text-white " @click="ataque">ATAQUE</div>
            <div class="bg-amber-400 hover:bg-amber-500 p-4 rounded-lg ">ATAQUE ESPECIAL</div>
            <div class="bg-lime-400 hover:bg-lime-500 p-4 rounded-lg text-white">CURAR</div>
            <div class="bg-gray-500 hover:bg-gray-600 p-4 rounded-lg " @click="jogoStart">DESISTIR</div>
        </div>

    </div>
</template>

<style scoped></style>
