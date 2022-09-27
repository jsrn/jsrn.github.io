
Game State

In previous chapters we discussed objects and the messages they send and receive. It would be unfair to continue without highlighting another key feature of objects. Along with methods, an object can have attributes.

Think of yourself. You are not an object, but you do have attributes. You probably have an eye colour, you may own 0 or more pairs of shoes. You might even have a preferred animal. These are your attributes. If you were to describe yourself in a class, you might write the following:

class Me
  attr_reader :eye_colour
  attr_reader :pairs_of_shoes
  attr_reader :preferred_animal
end

To put it another way, an attribute is something an object knows about itself. The sum of an object's attributes is its state. Thus, the state of the game is the sum of everything that every object knows. We saw an example of this in the last chapter. The Game object had the attribute @steps_taken.

It is a common anti-pattern in game development to have much or all of your state and management of game flow in one class. It will often be called something like "GameManager". This is a tempting habit to fall in to because at first, all will be clear to you and your game will be easy to enhance. In the long term, this is the road to ruin. As your game grows, the meaning of the file becomes more obscure, and change more costly and damaging.

    Antipattern: A common approach to a problem that appears fitting but generally causes more problems than it solves.

Now, it is probable that you will have a process that knows a lot about a lot. It is still good to avoid this where possible. Let us look at an example.

Our game from before has a new twist. In the updated version, the developers have added the ability for the player to lose hit points.

class Game
  MAX_PLAYER_HITPOINTS = 100

  def initialize
    @player_hit_points     = MAX_PLAYER_HITPOINTS
    # ...
  end

  def hurt_player(amount)
    @player_hit_points -= amount
    kill_player if @player_hit_points < 1
  end

  def kill_player
    puts "Game over!"
    exit
  end

  def start_game
    # ...
  end
end

Game.new

Our wanderer can now be overcome by a wide range of threats. Since our game must know about our player, it seems reasonable that the Game class should be responsible for managing our player. However, our player is quite capable of managing themselves.

class Game
  def initialize
    @player = Player.new
    # ...
  end

  def start_game
    # ...
  end
end

def Player
  MAX_PLAYER_HITPOINTS = 100

  def initialize
    @hit_points = MAX_PLAYER_HITPOINTS
  end

  def hurt(amount)
    @hit_points -= amount
    kill if @hit_points < 1
  end

  def kill
    # ...
  end
end

Game.new

We now have a player class that is largely responsible for managing its own state. The game class knows that the player exists and what it does, but how it does it is not the game class' concern. Trust between objects is a critical part of good object oriented design. By designing objects that can trust eachother to carry out their end of the bargain, we can develop a system that is easy to change, extend and maintain.

At all stages it is vital to carefully consider the structure that you wish to use to store your game's state.
