<script setup lang="ts">
import { ref } from "vue"
import BoardSquare from "./components/BoardSquare.vue"

type Player = {
    name: string
    symbol: string
}

type Square = {
    row: number
    col: number
    symbol: string
    highlight?: boolean
}

type Board = Array<Square>

const board = ref<Board>([
    { row: 0, col: 0, symbol: "" },
    { row: 0, col: 1, symbol: "" },
    { row: 0, col: 2, symbol: "" },
    { row: 1, col: 0, symbol: "" },
    { row: 1, col: 1, symbol: "" },
    { row: 1, col: 2, symbol: "" },
    { row: 2, col: 0, symbol: "" },
    { row: 2, col: 1, symbol: "" },
    { row: 2, col: 2, symbol: "" },
])

// const winSquares = ref<Array<Square>>([])

const playerOne: Player = {
    name: "Player One",
    symbol: "X",
}

const playerTwo: Player = {
    name: "Player Two",
    symbol: "O",
}

const currentPlayer = ref(playerOne)

const turn = (square: Square, player: Player) => {
    if (square.symbol !== "") return

    square.symbol = player.symbol
    currentPlayer.value =
        player.symbol === playerOne.symbol ? playerTwo : playerOne
    console.table(board.value)
    console.log(currentPlayer.value)

    // winSquares.value = getWinSquares()
    // if (winSquares.value.length > 0) {
    //     console.log(`Winner! ${currentPlayer.value.name}`)
    //     console.table(winSquares.value)
    // }
}
</script>

<template>
    <main>
        <h1 class="text-4xl">Tic-Tac-Toe</h1>
        <hr class="my-4" />
        <h2 class="text-2xl">
            Turn: {{ currentPlayer.name }} ({{ currentPlayer.symbol }})
        </h2>
        <div class="grid grid-cols-3 gap-1 w-48">
            <BoardSquare
                v-for="square in board"
                @click="turn(square, currentPlayer)"
                :value="square.symbol"
                :highlight="square.highlight"
            />
        </div>
    </main>
</template>
