# group_assignment1

# members

#   - Nzaramyimana Frodouard                 -->  221015977

#   - Umubyeyi Uwase Sandrine                --> 221025801

#   - Umuvandimwe Jean Marie Vianney         --> 221011017


link to git repository:   


# path to exact assignment aswer file   :    github_group_assignment1/group_ass

#  file name :   assignmentfile.kt








# codes solution to assignment available below


package group_ass

class GuessColorGame(val colors: List<Char>) {
    fun Feedback(player1Code: List<Char>, guess: List<Char>): Pair<Int, Int> {
        var setposition = 0
        var setcolor = 0

        for (i in player1Code.indices) {
            if (player1Code[i] == guess[i]) {
                setposition++
            } else if (player1Code.contains(guess[i])) {
                setcolor++
            }
        }

        return Pair(setposition, setcolor)
    }

    fun getPlayerGuess(): List<Char> {
        println("you should enter codes in this Available colors: $colors")
        println("Enter your guess (e.g., RGOW, RRWO , ......): ")
        val guess = readLine()?.uppercase() ?: ""
        if (guess == null || guess.length != 4 || guess.any { it !in listOf('R', 'B', 'Y', 'G', 'O', 'W') }) {
            println("Invalid input. Please enter a valid guess.")

        }

        return guess.filter { it in colors }.toList()
    }
}

class Setter(private val playerName: String, private val colors: List<Char>) {
    fun setguessCode(): List<Char> {
        println("$playerName, set the code and player two is going to guess it until correct guess is made")
        return GuessColorGame(colors).getPlayerGuess()
    }
}

class Guesser(private val colors: List<Char>) {

    fun playGame(playerName: String): Boolean {
        var contineu = true
        var attempts = 0

        while (contineu) {
            val guessCode = Setter(playerName, colors).setguessCode()
            attempts = 0

            while (true) {
                val guess = GuessColorGame(colors).getPlayerGuess()
                val feedback = GuessColorGame(colors).Feedback(guessCode, guess)

                println("Attempt $attempts - Feedback: " +
                        "${feedback.first} correct color(s) in correct position, " +
                        "${feedback.second} correct color(s) in wrong position.")

                if (feedback.first == 4) {
                    println("$playerName, congratulations! You guessed the correct code in $attempts attempts.")
                    contineu = askToContinue()
                    break
                }

                attempts++
            }
        }

        return contineu
    }


}

private fun askToContinue(): Boolean {
    println("Player 2 !! Do you want to continue? (Y/N)")
    val response = readLine()?.toLowerCase() ?: ""
    return response == "y"
}


fun main() {

    println("Welcome to Correct Order Color Guess Game! please follow instructions clearly !!!")

    val colors = listOf('R', 'B', 'Y', 'G', 'O', 'W')

    println("(Player 1) : Enter the your Name :")
    val player1Name = readLine() ?: "player 1"
    println("(Player 2) : Enter the your Name :")
    val player2Name = readLine() ?: "player 2"

    val codeGuesser = Guesser(colors)

    var contineu= true

    while (contineu) {
        contineu = codeGuesser.playGame(player1Name)
    }

    println(" $player2Name i'm sorry to see you leave please came back !, $player1Name is happy for you \uD83E\uDD70\uD83E\uDD70")
}
