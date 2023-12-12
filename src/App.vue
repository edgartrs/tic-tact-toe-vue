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

const winSquares = ref<Array<Square>>([])

const playerOne: Player = {
    name: "Player One",
    symbol: "X",
}

const playerTwo: Player = {
    name: "Player Two",
    symbol: "O",
}

const currentPlayer = ref(playerOne)
const winner = ref<Player | null>(null)

const turn = (square: Square, player: Player) => {
    if (winner.value) return
    if (square.symbol !== "") return

    square.symbol = player.symbol
    currentPlayer.value =
        player.symbol === playerOne.symbol ? playerTwo : playerOne
    console.table(board.value)
    console.log(currentPlayer.value)

    winSquares.value = getWinSquares()
    if (winSquares.value.length > 0) {
        winner.value = currentPlayer.value
        console.log(`Winner! ${currentPlayer.value.name}`)
        console.table(winSquares.value)
    }
}

const reset = () => {
    board.value.forEach((square) => {
        square.symbol = ""
        square.highlight = false
    })
    winner.value = null
    currentPlayer.value = playerOne
}

const getWinSquares = () => {
    const winSquares: Array<Square> = []

    // Check rows
    for (let row = 0; row < 3; row++) {
        const currentSymbol = board.value[row].symbol
        const rowSquares = board.value.filter((square) => square.row === row)
        if (
            rowSquares.every(
                (square) =>
                    square.symbol === currentSymbol && currentSymbol !== ""
            )
        ) {
            winSquares.push(...rowSquares)
        }
    }

    // Check columns
    for (let col = 0; col < 3; col++) {
        const currentSymbol = board.value[col].symbol
        const colSquares = board.value.filter((square) => square.col === col)
        if (
            colSquares.every(
                (square) =>
                    square.symbol === currentSymbol && currentSymbol !== ""
            )
        ) {
            winSquares.push(...colSquares)
        }
    }

    // Check diagonals
    const diagonalSquares = board.value.filter(
        (square) => square.row === square.col
    )
    const diagSymbol = diagonalSquares[0].symbol
    if (
        diagonalSquares.every(
            (square) => square.symbol === diagSymbol && diagSymbol !== ""
        )
    ) {
        winSquares.push(...diagonalSquares)
    }

    const antiDiagSymbol = board.value[2].symbol
    const antiDiagonalSquares = board.value.filter(
        (square) => square.row + square.col === 2
    )
    if (
        antiDiagonalSquares.every(
            (square) =>
                square.symbol === antiDiagSymbol && antiDiagSymbol !== ""
        )
    ) {
        winSquares.push(...antiDiagonalSquares)
    }

    // Highlight winning squares
    if (winSquares.length > 0) {
        winSquares.forEach((square) => {
            square.highlight = true
        })
    }

    return winSquares
}
</script>

<template>
    <main class="flex justify-center items-center">
        <div class="mx-auto">
            <h1 class="text-4xl">Tic-Tac-Toe</h1>
            <hr class="my-4" />
            <div class="mx-auto">
                <button
                    class="bg-slate-300 text-gray-900 rounded-lg p-2 hover:bg-slate-400 focus:outline-none focus:ring-2 focus:ring-slate-500"
                    @click="reset"
                >
                    New Game
                </button>
                <h2 class="text-l mt-8">
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
            </div>
        </div>
    </main>
</template>
