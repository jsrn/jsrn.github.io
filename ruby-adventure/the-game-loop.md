
The Game Loop

The game loop is an important concept in any game, from the most low tech pong clong to a triple A graphics card melting FPS. An oversimplified version has three stages:

    Take input from the player.
    Change game state accordingly.
    Display something to the player.

The size and scope of the game will drastically alter the amount of work that goes into step 2, but the concept remains the same.

The game we're going to start with is incredibly basic. It's about a lone wanderer. He walks. He walks a lot. That's all he does.

class Game
  def initialize
    @steps_taken = 0
    start_game
  end

  def start_game
    while true
      puts "You have taken #{@steps_taken} steps."
      @steps_taken += 1
    end
  end
end

Game.new

The observant reader will realise that the above code only truly performs steps 2 and 3 of the game loop.

Step two is to update the game state. We see that the only thing that currently qualifies is the variable @steps_taken. Step three is output, which is handled entirely by the call to #puts.

Perhaps we want to change our game loop so that it includes step 1, taking user input.

while true
  puts "You have taken #{@steps_taken}"
  steps_to_take = gets.chomp.to_i
  @steps_taken += steps_to_take
end

Now we have a complete, albeit very simple, game loop. If you are following along, you just made your first text adventure! Unfortunately, it gets very boring, very quickly. It has no progression. For a game to advance from one stage to the next in a meaningful way, it must have some idea of its own state.
Work Prompts

    What kind of loop might be best for the game loop? It depends on what your criteria are for the game being over. Try writing a version of the above game that ends after 100 steps.
    Write a simple game loop that takes input from the player and responds with a message.

