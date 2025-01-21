<script setup lang="ts">
import { ref } from 'vue';

const vidaJogador = ref('100%');
const vidaMonstro = ref('100%');
</script>


<template>
    <div class="m-10 p-10 border-2 flex justify-center space-x-7">
        <div id="jogador" class=" flex flex-col space-y-14 items-center border-2 w-1/2 justify-center">
            <h1 class="text-6xl">Jogador</h1>

            <div class="w-full flex flex-col items-center">
                <div class="h-8 w-9/12 border-2 border-gray-400">
                    <div :class="parseInt(vidaJogador) > 20 ? 'bg-lime-600' : 'bg-red-700'" class="h-full" :style="{ width: vidaJogador }"></div>

                </div>
                <p class="text-3xl">{{ vidaJogador }}</p>
            </div>

        </div>
        <div id="jogador" class=" flex flex-col space-y-14 items-center border-2 w-1/2 justify-center">
            <h1 class="text-6xl">Monstro</h1>

            <div class="w-full flex flex-col items-center">
                <div class="h-8 w-9/12 border-2 border-gray-400">
                    <div :class="parseInt(vidaMonstro) > 20 ? 'bg-lime-600' : 'bg-red-700'" class="h-full" :style="{ width: vidaMonstro}"></div>

                </div>
                <p class="text-3xl">{{ vidaMonstro }}</p>
            </div>

        </div>
    </div>
</template>

<style scoped></style>
