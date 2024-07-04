<template>
  <main class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-white">
      <h1 class="mb-8 text-3xl font-bold uppercase">Jeu Tic Tac Toe</h1>
      <h3 class="text-xl mb-4">C'est  le tour du joueur {{ player }}</h3>

      <div class="flex flex-col items-center mb-8">
            <div 
                v-for="(row, x) in board"
                :key="x"
                class="flex">

                    <div
                          v-for="(cell, y) in row"
                          :key="y"
                          @click="MakeMove(x,y)"
                          :class="`border border-white w-20 h-20 hover:bg-gray-700 
                          flex items-center justify-center material-icons-outlined
                          text-4xl cursor-pointer ${ cell === 'X' ? 'text-pink-500' : 'text-blue-400'}`">
                          {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : ''}}
                    </div>
            </div>
      </div>
      <h2 v-if="winner" class="text-6xl font-bold mb-8">Joueur '{{ winner }}' gagne cette partie!</h2>

      <button @click="ResetGame" class="px-4 py-2 bg-pink-500 rounded 
                font-bold hover:bg-pink-600 duration-300">Recommencer la partie
      </button>
  </main>
</template>




<script setup>
import {ref,computed} from 'vue'

   const player = ref ('X');

   //notre tableau dans lequel on coche les cases
   const board = ref([
        ['','',''],
        ['','',''],
        ['','',''],
   ]);

//premier comit
 const CalculateWinner = (squares) => {

    //on defini un tableau contenant toutes les lignes,colonnes,diagonales 
    //qui pourrait constituer une victoire dans le jeu
    const lines = [
      [0,1,2],
      [3,4,5],
      [6,7,8],
      [0,3,6],
      [1,4,7],
      [2,5,8],
      [0,4,8],
      [2,4,6],
    ];

    //Pour chaque element de notre tableau lines (lignes,colonnes,diagonales)
    for (let i = 0; i< lines.length; i++){
    //on va utiliser la déstructuration de javascript pour extraire les 3 elements de l'element lines(i)
    //et les stoccker dans les variables a,b,c
      const [a, b, c] = lines[i];
      //squares est un tableau qui représente les 9 cases du jeu 
      //si 1er element dans 1 case es égale au 2nd element dans une autre case 
      //et que 1er element = 3eme element dans autre case et sa respect notre tableau lines victoire
      if(squares[a] && squares[a] === squares[b] && squares[a] === squares[c]){
        return squares[a];
      }
    }
    return null;
   }

   //board.value.flat() : board est une référence réactive à un tableau à deux dimensions représentant le plateau
   // de jeu du morpion. 
   //.value est utilisé pour accéder à la valeur actuelle de la référence réactive, qui est un tableau à deux dimensions.
   // .flat() est une méthode qui permet de transformer un tableau à plusieurs dimensions en un tableau à une seule dimension.
   // Ainsi, board.value.flat() donne un tableau à une dimension représentant les cases du jeu.
   //computed prend en argument une fonction qui recoit en argument CalculateWinner qui va retourner une reponse calculer
   const winner = computed (()=> CalculateWinner(board.value.flat()))

   // x,y représentent les coordonnées de la case dans laquelle le joueur souhaite placer ('X' ou 'O').
   const MakeMove = (x,y) => {
    //on vérifie s'il y a déjà un gagnant. Si oui la fonction s'arrête et ne permet pas au joueur de faire un mouvement supplémentaire.
    if (winner.value)return
    //on vérifie si la case spécifiée par (x, y) est déjà occupée par ('X' ou 'O'). Si oui, la fonction s'arrête et n'effectue pas le mouvement.
    if(board.value[x][y] !== '')return

    //si pas de joueur et case vide place le symbole du joueur
    board.value[x][y] = player.value

    //player.value 7 le joueur actuel, initialement défini à 'X' ou 'O'.
    //player.value est égal à 'X'. Si c'est le cas, elle renvoie 'O', sinon elle renvoie 'X'. 
    //Cela signifie que si le joueur actuel est 'X', il devient 'O' et vice versa
    //Pour permettre au 2 joueurs de jouer
    player.value = player.value === 'X' ? 'O' : 'X'
   }

   //cette fonction réinitialise le jeu et le joueur 
   const ResetGame = () => {
    board.value = [
        ['','',''],
        ['','',''],
        ['','',''],
    ]
    player.value = 'X'

   }
</script>


<style>

</style>
