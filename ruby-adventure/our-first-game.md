---
layout: adventure
title: "Ruby Adventure: Our first game"
---

Previous: [Structuring an application](/ruby-adventure/structuring-an-application)

## Our first game

Up until this point, our discussion has been more about Ruby and less about the game we are going to build in it. We have now covered enough material that we can begin pulling it together and create the bare bones of our game.

We're going to start with a basic (compared to a AAA game studio, at least) set of requirements:

- There's a player who starts the game on square 0,0 with 100 hit points (HP) and 1 attack power (AP).
- The world will be a 10x10 square. Every room is accessible by every adjacent room and every room contains either an item or a monster.
- Monsters have 10HP and 1AP.
- Available items are 'potion' which heals 10HP or 'a better sword' which grants the player an extra 1AP.
- When you enter a square with a monster, combat begins automatically. Every turn, the player does n AP worth of damage to the monster and the monster does 1AP worth of damage back.
- The game ends when all monsters are defeated or the player's HP reaches zero.

### Creating our player

```ruby
class Player
  attr_accessor :hit_points, :attack_power
  attr_accessor :x_coord, :y_coord

  MAX_HIT_POINTS = 100

  def initialize
    @hit_points        = MAX_HIT_POINTS
    @attack_power      = 1
    @x_coord, @y_coord = 0, 0
  end

  def alive?
    @hit_points > 0
  end

  def hurt(amount)
    @hit_points -= amount
  end

  def heal(amount)
    @hit_points += amount
    @hit_points = [@hit_points, MAX_HIT_POINTS].min
  end

  def print_status
    puts "*" * 80
    puts "HP: #{@hit_points}/#{MAX_HIT_POINTS}"
    puts "AP: #{@attack_power}"
    puts "*" * 80
  end
end
```

### Describing our items

```ruby
class Item
  TYPES = [:potion, :sword]

  attr_accessor :type

  def initialize
    @type = TYPES.sample
  end

  def interact(player)
    case @type
    when :potion
      puts "You pick up #{self}."
      player.heal(10)
    when :sword
      puts "You pick up #{self}."
      player.attack_power += 1
    end
  end

  def to_s
    "a shiny awesome #{@type.to_s}"
  end
end
```

### Creating a monster

```ruby
class Monster
  attr_accessor :hit_points, :attack_power

  MAX_HIT_POINTS = 10

  def initialize
    @hit_points   = MAX_HIT_POINTS
    @attack_power = 1
  end

  def alive?
    @hit_points > 0
  end

  def hurt(amount)
    @hit_points -= amount
  end

  def to_s
    "a horrible monster! garurururu"
  end

  def interact(player)
    while player.alive?
      puts "You hit the monster for #{player.attack_power} points."
      hurt(player.attack_power)
      break unless alive?
      player.hurt(@attack_power)
      puts "The monster hits you for #{@attack_power} points."
    end
  end
end
```

### The Game World

As said before, our world is a fixed 10 x 10 grid. This would be a great use case for the arrays we learned about back in chapter 1.

```ruby
[
  [room], [room], [room], # ...
  [room], [room], [room], # ...
  [room], [room], [room], # ...
  [room], [room], [room], # ...
  # ...
]
```

Above we have the two-dimensional array representing our game world.

```ruby
class World
  WORLD_WIDTH  = 10
  WORLD_HEIGHT = 10

  def initialize
    @rooms = Array.new(WORLD_HEIGHT, Array.new(WORLD_WIDTH))
  end

  def move_entity_north(entity)
    entity.y_coord -= 1 if entity.y_coord > 0
  end

  def move_entity_south(entity)
    entity.y_coord += 1 if entity.y_coord < WORLD_HEIGHT - 1
  end

  def move_entity_east(entity)
    entity.x_coord += 1 if entity.x_coord < WORLD_WIDTH - 1
  end

  def move_entity_west(entity)
    entity.x_coord -= 1 if entity.x_coord > 0
  end

  def get_room_of(entity)
    @rooms[entity.x_coord][entity.y_coord] ||= Room.new
  end
end

class Room
  attr_accessor :size, :content

  def initialize
    @content   = get_content
    @size      = get_size
    @adjective = get_adjective
  end

  def interact(player)
    if @content
      @content.interact(player)
      @content = nil
    end
  end

  def to_s
    "You are in a #{@size} room. It is #{@adjective}."
  end

  private
  def get_content
    [Monster, Item].sample.new
  end

  def get_size
    ["small", "medium", "large"].sample
  end

  def get_adjective
    ["pretty", "ugly", "hideous"].sample
  end
end
```

Putting it All Together

We now have our core objects floating around, in isolation. We have fleshed out the basics of what objects we expect to need and the messages we anticipate having to send and receive.

```ruby
Dir["lib/**.*"].each { |file| require_relative file }

class Game
  ACTIONS = [
    :north, :east, :south, :west, :look, :fight, :take, :status
  ]

  def initialize
    @world = World.new
    @player = Player.new

    start_game
  end

  private
  def start_game
    while @player.alive?
      @current_room = @world.get_room_of(@player)

      print_status

      action = take_player_input
      next unless ACTIONS.include? action

      take_action(action)
    end
  end

  def take_player_input
    print "What's the plan, Stan? "
    gets.chomp.to_sym
  end

  def print_status
    puts "You are at map coordinates [#{@player.x_coord}, #{@player.y_coord}]"

    puts @current_room
    if @current_room.content
      puts "You see #{@current_room.content}."
    end
  end

  def take_action(action)
    case action
    when :look
      print_status
    when :north
      @world.move_entity_north(@player)
    when :east
      @world.move_entity_east(@player)
    when :south
      @world.move_entity_south(@player)
    when :west
      @world.move_entity_west(@player)
    when :fight, :take
      @current_room.interact(@player)
    when :status
      @player.print_status
    end
  end
end

Game.new
```

There it is. A complete game with exploration, monsters, combat, and loot. It's not perfect, or even particularly enjoyable, but it holds the features we expected. Now, I will use the code we've just written to illustrate some problems in the design and how we can write better Ruby code.


### Identifying Roles

When you are developing a system in which objects are interacting, the idea of roles are central. You pass one object to another, and you expect that object to be able to be treated a certain way.
Expressing Roles Through Inheritance

Many object oriented languages allow something called inheritance. A class can inherit the behaviour of another, taking on all of its methods and attributes.

```ruby
class Parent
  def speak
    puts "Hello, world!"
  end
end

class Child < Parent
end

child = Child.new
child.speak
# Hello, world!
```

Whether you realise it or not, you have already been using classes that inherit from others. Earlier, I expressed the idea that everything in Ruby #is_a? Object. This is because everything, directly or indirectly, inherits from Ruby's Object class.

You should apply inheritance in Ruby when you realise that you have multiple objects that are the same category of thing, but with minor variations. Another time you may find it useful to apply inheritance is when you have a single object that switches behaviour based on some value. A good example of this is our Item class. Take a look at the offending lines:

case @type
when :potion
  puts "You pick up #{self}."
  player.heal(10)
when :sword
  puts "You pick up #{self}."
  player.attack_power += 1
end

When the user interacts with one of these items, an effect is applied depending on the item's type property. This is bad for two reasons:

    It is not manageable in the long term. Look at the code with two variations, and imagine it with 10 item types. Imagine it with 100. Imagine it with 1,000. It's not pretty.
    The Item class no longer represents an item. It represents all items, and all manner of behaviour.

The only difference between one item type and another is the name, and the effect when applied. It makes sense to extract these into interchangeable Item subclasses.

class Item
  def interact(player)
    puts "You pick up #{self}"

    perform_item_effect(player)
  end

  def to_s
    "a shiny awesome #{@name}"
  end
end

class Potion < Item
  def initialize
    @name = "potion"
  end

  def perform_item_effect(player)
    player.heal(10)
  end
end

class Sword < Item
  def initialize
    @name = "sword"
  end

  def perform_item_effect(player)
    player.attack_power += 1
  end
end

Now, as long as our item classes implement the #perform_item_effect method that is called in the #initialize method of the Item class, they are interchangeable.
Expressing Roles Through Modules

The clearest example of this is in the resolution of combat which, rightly or wrongly, currently resides in the Monster class.

while player.alive?
  puts "You hit the monster for #{player.attack_power} points."
  hurt(player.attack_power)
  break unless alive?
  player.hurt(@attack_power)
  puts "The monster hits you for #{@attack_power} points."
end

Here we have an epic battle between two combatants. They're both capable of dishing out damage via their attack_power attribute and taking it via their #hurt methods. We've discovered common behaviour between our combatants. We've uncovered a role. We want to share behaviour between two objects which are clearly different. This is where Ruby modules (see chapter 1) come in. We can extract this shared functionality to a module that will allow both our Player and Monster to engage in combat. Since it is the convention amongst Ruby programmers to name their mix-in modules as adjectives, we shall call our module Combatable. If you look back at our Player and Monster classes, the shared behaviour should be pretty obvious. Let's extract it to our module.

module Combatable
  BASE_STATS = {
    max_hit_points: 10,
    attack_power:   1
  }

  def Combatable.included(mod)
    attr_accessor :hit_points, :attack_power
  end

  def initialize_stats(stats)
    @stats = stats

    @hit_points   = stats[:max_hit_points]
    @attack_power = stats[:attack_power]
  end

  def alive?
    @hit_points > 0
  end

  def hurt(amount)
    @hit_points -= amount
  end

  def heal(amount)
    @hit_points += amount
    @hit_points = [@hit_points, @stats[:max_hit_points]].min
  end
end
end

Now take a look at how we can change our Player class to accomodate this

require_relative "combatable.rb"

class Player
  include Combatable
  attr_accessor :x_coord, :y_coord

  MAX_HIT_POINTS = 100

  def initialize
    initialize_stats(BASE_STATS.merge ({
      max_hit_points: MAX_HIT_POINTS
    }))

    @x_coord, @y_coord = 0, 0
  end

  def print_status
    puts "*" * 80
    puts "HP: #{@hit_points}/#{MAX_HIT_POINTS}"
    puts "AP: #{@attack_power}"
    puts "*" * 80
  end
end

As you can see, the Player class no longer has to directly confront its own mortality. Now that we have identified the behaviour of our combat system, we can include it in any class that we wish to enter the arena.
