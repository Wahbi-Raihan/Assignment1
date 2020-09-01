# Assignment 1 - Java Programming

## Identification

Your name:  `INSERT YOUR NAME HERE`

Your teammates name:  `INSERT YOUR TEAMMATES NAME HERE (if you have one)`

## Collaboration Policy
Students are encouraged to work in teams of two students for this assignment. Students can work alone, if they prefer.  Teams larger than 2 are not permitted.

## Overview
For this assignment, we will create a simple battle system for a JRPG style game.  The battle system will simulate a battle between a human player and a CPU player.  Each player will have their 'monster' that will battle one another using a selection of moves.

_**Note:** Files already exist for the following classes:_
- `src/main/java/GameDriver.java`
- `src/main/java/Player.java`
- `src/main/java/HumanPlayer.java`
- `src/main/java/CPUPlayer.java`
- `src/main/java/Monster.java`
- `src/main/java/Move.java`

## Part 1
Your classes should work given any configuration of players and monsters.  An example driver class (`GameDriver`) that tests the various classes that you will eventually implement has been provided, below:

```
public class GameDriver {
	public static void main(String[] args) {
		Move move1 = new Move("Vine Whip", "Grass", 45, 1.0f);
		Move move2 = new Move("Tackle", "Normal", 50, 1.0f);
		Move move3 = new Move("Take Down", "Normal", 90, 0.85f);
		Move move4 = new Move("Razor Leaf", "Grass", 55, 0.95f);
		Monster monster = new Monster("Bulbasaur", "Grass", 240, 45, 49, 49, 
                                      move1, move2, move3, move4);
		Player player = new HumanPlayer(monster);

		move1 = new Move("Scratch", "Normal", 40, 1.0f);
		move2 = new Move("Ember", "Fire", 40, 1.0f);
		move3 = new Move("Peck", "Flying", 35, 1.0f);
		move4 = new Move("Fire Spin", "Fire", 35, 0.85f);
		monster = new Monster("Torchic", "Fire", 240, 45, 60, 40, 
                              move1, move2, move3, move4);
		Player enemy = new CPUPlayer(monster);

		while ((!player.hasLost()) && (!enemy.hasLost())) {
			// print both monsters' HP
			System.out.println("");
			System.out.printf("%s has %d HP\n", 
                                    player.getMonster().getName(), 
                                    player.getMonster().getHP());
			System.out.printf("%s has %d HP\n",                
                                    enemy.getMonster().getName(), 
                                    enemy.getMonster().getHP());
			
			// decide the next move
			int playerMove = player.chooseMove();
			int enemyMove = enemy.chooseMove();
			
			// execute the next move
			if (player.isFasterThan(enemy)) {
				player.attack(enemy, playerMove);
				if (!enemy.hasLost()) {
					enemy.attack(player, enemyMove);
				}
			} else {
				enemy.attack(player, enemyMove);
				if (!player.hasLost()) {
					player.attack(enemy, playerMove);
				}
			}
		}
		
		if (player.hasLost()) {
			System.out.printf("You and %s have lost the battle.\n", 
                                    player.getMonster().getName());
		} else {
			System.out.printf("You and %s are victorious!\n", 
                                    player.getMonster().getName());
		}
	}
}
```

Upon examining this driver code, you will see that you need to implement the following classes:
- `Player` - represents players in general
    - This class has no constructor
- `HumanPlayer` - selects moves by asking the user via console input
    - Constructor arguments:
        - `monster` (`Monster`) - the player's battling monster
- `CPUPlayer` - selects moves by generating a random number
    - Constructor arguments:
        - `monster` (`Monster`) - the player's battling monster
- `Monster` - represents a monster with four available moves
    - Constructor arguments:
        - `name` (`String`) - the name of the creature
        - `type` (`String`) - the type of the creature
        - `hp` (`int`) - the hit points of the creature
        - `speed` (`int`) - the speed stat of the creature
        - `attack` (`int`) - the attack stat of the creature
        - `defense` (`int`) - the defense stat of the creature
        - `move1` (`Move`) - the first move
        - `move2` (`Move`) - the second move
        - `move3` (`Move`) - the third move
        - `move4` (`Move`) - the fourth move
- `Move` - represents a single move
    - Constructor arguments:
        - `name` (`String`) - the name of the move
        - `type` (`String`) - the type of the move
        - `power` (`int`) - the attack power of the move
        - `accuracy` (`float`) - `0.0`-`1.0`, representing the hit percentage for the move

It is permitted for you to create any additional classes required to implement the correct behaviour, as long as the _game_ still launches via the `GameDriver` class, like it currently does.

The battle system first asks each player to choose their next move.  The CPU player will select their move randomly.  The human player will present a list of moves to the user on the console, and ask the user to select one (e.g. by entering a number 1-4) on the console.  The battle system then selects which monster is faster (a monster with a higher speed stat is faster).  The faster monster uses their move first.

Then, determine if the move will hit the defending monster.  Generate a random floating point number between 0.0 and 1.0.  If the number is greater than the accuracy of the move (which is impossible for some moves that have accuracy 1.0), then the move is a miss and no damage is dealt to the defending monster.

Each move's attack is calculated using the following formula:

```
damageDealth = attacking monster's attack stat +
               attacking monster move's power -
               defending monster's defense stat
```

The calculated damage is then subtracted from the HP of the defending monster.  If the HP becomes less than (or equal to) zero, the monster is dead and cannot attack any further, including the move selected at the beginning of this round.

If both monsters are still alive when the round has finished, the process repeats.  When finished, the driver class will display a message determining if the human player has won or lost the battle.

## Part 2
Determine how all of your classes are related, and create a complete UML class diagram representing your class structure.  Don't forget to include the appropriate relationships between the classes.

Note:  You can use any tool you want to generate UML.  If you don't know what to use, here are a few suggestions:
- Violet (http://alexdp.free.fr/violetumleditor/page.php) – a free, open source UML editor
- Dia (https://wiki.gnome.org/action/show/Apps/Dia?action=show&redirect=Dia) – a free, open-source diagramming tool (including UML)
- UMLet (http://www.umlet.com/) - a free UML editor
- yUML (http://yuml.me/) - a free-to-use UML editor (with commercial offerings)

Export your diagram to an image (`.png`), before you submit, to ensure that the instructor and/or TA can open the file easily.

_**Hint:**  This part is likely just a one-page diagram._

## Part 3
Add some additional functionality of your choice to the battle system.  Be creative!  Choose something that is doable, but challenging enough to demonstrate the skills you have gained in this course.  You could implement an enemy monster AI, a graphical display, anything as long as it allows you to demonstrate several of the course concepts that we've learned so far.

## Compiling and Running Your Program
To compile and run your program, use the following command:
`gradle run -q --console=plain`

_**Note:** The `-q` and `--console=plain` are to suppress the extra output that gradle normally prints when running a Java application._

## Submission Instructions
Modify the Java classes as described in this document, and add any additional classes you decide you need to write to complete this lab assignment.  Commit and push your code to this repository.

The late assignment policy in this course is a deduction of 10% of the maximum grade for every day late, up to 3 days.  After 3 days, the assignment will not be accepted.

This repository will be marked by the TA or instructor at their convenience, but any changes made to this repository after the due date (described above) will not be considered automatically.  If you wish those changes to be marked, and the corresponding late penalty to take effect, please communicate your wishes to the instructor.

## Getting Help
If you run into difficulty, you may wish to check out some of the following resources:
- https://docs.oracle.com/javase/tutorial/tutorialLearningPaths.html - A series of tutorials for the Java programming language, focusing almost entirely on the features of Java
- https://docs.oracle.com/en/java/javase/14/ - The standard documentation for Java classes, including methods that you can use, some of which will be discussed later in this course
- http://stackoverflow.com/ - A forum for asking questions about programming. Chances are, someone else has asked the same question as you have, and some knowledgeable person has already answered it.  This might be a good time to use the ‘site:’ feature in Google!

Of course, you can always ask the TAs or the instructor for help! However, learning how to find the answers out for yourself is a skill that will pay off in the future, as solving your own problem is immediate (and satisfying)!

## Academic Integrity
Discussing strategies with your fellow students is acceptable, but once it is time to write the code you should do so on your own (or with only your group).  The instructor has learning goals planned for this course which are cumulative.  If you fail to learn some elements in this assignment, it most likely will affect your performance on higher-stakes assessments in the future.  You can also ask the TA or the instructor for help, but they won't directly solve your problems for you, but will rather point you in the right direction to find the solution yourself.