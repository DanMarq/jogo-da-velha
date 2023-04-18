<script setup>
import { computed, ref } from 'vue';

const player = ref('X')
const board = ref([
    ['','',''],
    ['','',''],
    ['','','']
])

const gameOver = ref(false)

const checkForWin = (combo) => {
    const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
    ]

    for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i]
        if (combo[a] && combo[a] === combo[b] && combo[a] === combo[c]) {
            return combo[a]
        } 
    } 
    return null
}

const winner = computed(() => checkForWin(board.value.flat()))

const checkForDraw = () => {
    const flatBoard = board.value.flat()
    return flatBoard.every(cell => cell) && !winner.value
}


const MakeMove = (x, y) => {
    if (winner.value || gameOver.value) return

    if (board.value[x][y]) return

    board.value[x][y] = player.value

    if (checkForWin(board.value.flat())) {
        gameOver.value = true
        return
    }

    if (checkForDraw()) {
        gameOver.value = true
        return
    }

    player.value = player.value === 'X' ? 'O' : 'X'
}

const ResetGame = () => {
    board.value = [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
    ]

    player.value = 'X'
    gameOver.value = false
}

</script>

<template>
    <main class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-white transition-all duration-500">
        <h1 class="mb-8 text-4xl font-bold uppercase">Jogo da Velha</h1>
        <h3 class="text-3xl mb-6"> É a vez do jogador <span :class="`${player === 'X' ? 'text-pink-500' : 'text-blue-400'}`">{{ player }}</span></h3>

        <div class="flex flex-col items-center mb-8">
            <div v-for="(row, x) in board" :key="x" class="flex">
                <div v-for="(cell, y) in row" :key="y" @click="MakeMove(x, y)"
                :class="`border dark:border-white w-20 h-20 hover:dark:bg-gray-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${cell === 'X' ? 'text-pink-500' : 'text-blue-400'}`">
                    {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
                </div>
            </div>
        </div>

        <div v-if="gameOver" class="text-center">
            <h2 v-if="winner" class="text-6xl font-bold mb-8">jogador <span :class="`${player === 'X' ? 'text-pink-500' : 'text-blue-400'}`">{{ winner }}</span> venceu!!</h2>
            <h2 v-else class="text-6xl font-bold mb-8">Eita, deu empate!</h2>
            <button @click="ResetGame" class="px-4 py-2 bg-indigo-500 rounded uppercase font-bold hover:bg-indigo-600 duration-300 shadow-lg">Limpar o jogo</button>
        </div>
    </main>    
</template>