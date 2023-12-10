<script setup lang="ts">
import { ref } from "vue"
import BoardSquare from "./components/BoardSquare.vue"

type Player = {
    name: string
    symbol: string
}

const board = ref([
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
])

const playerOne: Player = {
    name: "Player One",
    symbol: "X",
}

const playerTwo: Player = {
    name: "Player Two",
    symbol: "O",
}

const currentPlayer = ref(playerOne)

const turn = (row: number, col: number, player: Player) => {
    if (board.value[row][col] !== "") return

    board.value[row][col] = player.symbol
    currentPlayer.value =
        player.symbol === playerOne.symbol ? playerTwo : playerOne
    console.table(board.value)
    console.log(currentPlayer.value)
}
</script>

<template>
    <main>
        <h1 class="text-4xl">Tic-Tac-Toe</h1>
        <hr class="my-4" />
        <h2 class="text-2xl">Current Player: {{ currentPlayer.name }}</h2>
        <div class="grid grid-cols-3 gap-1 w-48">
            <BoardSquare
                @click="turn(0, 0, currentPlayer)"
                :value="board[0][0]"
            />
            <BoardSquare
                @click="turn(0, 1, currentPlayer)"
                :value="board[0][1]"
            />
            <BoardSquare
                @click="turn(0, 2, currentPlayer)"
                :value="board[0][2]"
            />
            <BoardSquare
                @click="turn(1, 0, currentPlayer)"
                :value="board[1][0]"
            />
            <BoardSquare
                @click="turn(1, 1, currentPlayer)"
                :value="board[1][1]"
            />
            <BoardSquare
                @click="turn(1, 2, currentPlayer)"
                :value="board[1][2]"
            />
            <BoardSquare
                @click="turn(2, 0, currentPlayer)"
                :value="board[2][0]"
            />
            <BoardSquare
                @click="turn(2, 1, currentPlayer)"
                :value="board[2][1]"
            />
            <BoardSquare
                @click="turn(2, 2, currentPlayer)"
                :value="board[2][2]"
            />
        </div>
    </main>
</template>
