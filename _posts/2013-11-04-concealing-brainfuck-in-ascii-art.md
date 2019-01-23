---
layout: post
title: "Concealing Brainfuck in ASCII Art"
description: ""
category: programming
tags: [brainfuck, esoteric languages, ascii art]
---

[Brainfuck](http://en.wikipedia.org/wiki/Brainfuck) is a neat little language that gives you 8 commands and an instruction pointer. The entire language consists of these characters:

	><+-.,[]

As the name implies, the language is not designed for its readability. Here's Hello World:

	++++++++++[>+++++++>++++++++++>+++>+<<<<-]>++.>+.+++++++..+++.>++.<<+++++++++++++++.>.+++.------.--------.>+.>.

Since there's no defined standard for the language, different interpreters have slightly different behaviours, but generally, any other character is simply ignored.

Since the language is already very difficult to read, I thought it only made sense to make the situation worse.

{% highlight ruby %}
#!/usr/bin/env ruby

brainfuck = File.open("helloworld.bf", "rb").read

ascii_art = File.open("animaniacs.art", "rb").read

if brainfuck.length > ascii_art.gsub(" ","").length
  abort "The supplied brainfuck will not fit inside the ASCII art."
end

output = ""
ascii_art.split("").each do |i|
  if brainfuck != ""
    if i != " " and i != "\n"
      i = brainfuck[0,1]
      brainfuck = brainfuck[1..-1]
    end
  end
  output << i
end

File.open( "hidden.bf", "w" ) do |file|
  file.write( output )
end
{% endhighlight %}

The above code reads two files. The first is the file containing the plain BF code. The second contains the ASCII artwork that you would like to put the code inside.

Input:

	                 ____
	              d@@@@@@bo      @@_
	               ~~~~~ Y@@     Y@@_
	                   _o@@@@@b_  Y@@
	                  d@@@@@@@@@@b @P                     ___
	                 @P ~P~    @@@@P                    _d@@@P---__
	                dP          d@P                    / @P        -dY@@b
	             t=_@  d ab   _o@@~      ___          / @@           \ YP
	             |   - @_@~  _d@@@___##########_______|______         |
	             k _  (__)  ~~Y@@@F#####oo|oo#####_-~~  /@   ~~---____|
	            ( Y |\    _T   Y@@OOOOOOO~^~OOO_-~ ___Ao@@b   a    _@@) __
	             \  |_`--'     dP              \---   V     @_@   d@@@ (_ \   __
	           ___-_   ~-____-~                |      (   T (_)_T   Y@F  ) v~~__)
	        ########-___ )@@@b           _')    `_     \ ( \__'/     )##}k   {_
	       _###oodOOOP__)@@@@@b      __  ( \      ~--__ \\\n  //__--ooCCU\   _ \
	   __######OO    @@/@@@@@@@b    (_ ~~~ _}_         ~~\}~~v/     oCCP###| |\_)
	_######odOOOP   @@/@@@@@@@@@@b    ~~} / r)@CCCCCCoooo(   )  _ooCCC~Obo#\_###_
	###b\OOP~~     @@/@@@@@@@@@@@@@b _r~v'_'     ~~CCCCCCC\_/CCCCCCC~  ~~YOO/d###
	###OOO        @@/@@@P_}----_@@Y@(_}--'            CCCCCCCCCCCC~        OOO###
	`##*OO       @@@@@P ((__-^-__)U@@@@@             dCCCCCCCCCCC          OO*##'
	 ###OOb      @@@P____##################################*YCCCC         dOO###
	 `##OOO  ____###############| \##/ |#/  \###|  \#| |############____  OOO##'
	  ###OO*#############| |#| ||  \/  |/ /\ \##|   \| || |##/  \########*OO###
	 _#############|   ~-| |#| ||      | ____ \#| |\ \ || |#/ /\ \##/   )######_
	{ ######/  \###| |-__   |#| |_|\/|_|/####\ \| |#\  || |/ ____ \/ /###/ _ \##}
	 \#####/ /\ \##|  |##-__|#|_|#######@@###d@_#_|##\_||_|_/####\_| |##( (#\/#/
	  \###/ ____ \##| |#################Y@b_@@#####################\  )##\ \##/
	   \##_/####\_#######oooOOOOODROOOOOb@@@@dOOGUZOOOOOoooooo#########'\/ /#/
	   \#############OOOOO~~~~~        _-\  /-_       ~~~~~~~OOOOOoo###\__/##/
	    \########OOP~   _              (__ O _/                 ~~OOOOO#####/
	     \#dOOOOOO_    | Y~_            (__d__)                   _OOOOOOb#/
	      OO(###OOO_   | r_|    ____oood@@@@@@@@oooo@F           _OOO###)OO
	       ~~O###OOOo   Y ||   _Y@@@@@@P   ~V~  Y@@@@F          o000###O~~
	          ~`##*OOb  | r_\_/ _)@@@@@          @@@@f  __     _--*##'~
	            `##*OOb_ \  `_ / ~@@@@@    a a   @@@@  (_ `---'  ~~)'
	             ~###OOOo `-__Y)   @@@@b   @_@  d@@@     lv'~   ~~#~
	               `##*OOb   (_@b   Y@~ T  (_) T   )   d@Uknnn###'
	                ~###OOOb    Yb_  (  |\    /|  / od@P bdOO###~
	                  `###OOOb   Y@b  `-| `--'_'_d@@@P dOOO###'
	                   ~####OOOb   Y@b  \\nn //@@@@P dOOO####~
	                     ~####OOOb  ~@@@@\`-'/@@@P~dOOO####~
	                       ~####OOOb  Y@@@@@@@@@~dOOO####~
	                         ~####*OObo@@@@@@@odOO*####~
	                           ~~####*OOOb|dOOO*####~~
	                              ~~#####*|*#####~~
	                                  ~~#####~~                       
	                                      ~     Sean Gugler

Output:

	                 ++++
	              ++++++[>+      +++
	               +++>+ +++     ++++
	                   ++>+++>+<  <<<
	                  -]>++.>+.+++ ++                     ++.
	                 .+ ++.    >++.<                    <++++++++++
	                ++          +++                    . >.        +++.--
	             ----  . --   -----      -.>          + .>           . YP
	             |   - @_@~  _d@@@___##########_______|______         |
	             k _  (__)  ~~Y@@@F#####oo|oo#####_-~~  /@   ~~---____|
	            ( Y |\    _T   Y@@OOOOOOO~^~OOO_-~ ___Ao@@b   a    _@@) __
	             \  |_`--'     dP              \---   V     @_@   d@@@ (_ \   __
	           ___-_   ~-____-~                |      (   T (_)_T   Y@F  ) v~~__)
	        ########-___ )@@@b           _')    `_     \ ( \__'/     )##}k   {_
	       _###oodOOOP__)@@@@@b      __  ( \      ~--__ \\\n  //__--ooCCU\   _ \
	   __######OO    @@/@@@@@@@b    (_ ~~~ _}_         ~~\}~~v/     oCCP###| |\_)
	_######odOOOP   @@/@@@@@@@@@@b    ~~} / r)@CCCCCCoooo(   )  _ooCCC~Obo#\_###_
	###b\OOP~~     @@/@@@@@@@@@@@@@b _r~v'_'     ~~CCCCCCC\_/CCCCCCC~  ~~YOO/d###
	###OOO        @@/@@@P_}----_@@Y@(_}--'            CCCCCCCCCCCC~        OOO###
	`##*OO       @@@@@P ((__-^-__)U@@@@@             dCCCCCCCCCCC          OO*##'
	 ###OOb      @@@P____##################################*YCCCC         dOO###
	 `##OOO  ____###############| \##/ |#/  \###|  \#| |############____  OOO##'
	  ###OO*#############| |#| ||  \/  |/ /\ \##|   \| || |##/  \########*OO###
	 _#############|   ~-| |#| ||      | ____ \#| |\ \ || |#/ /\ \##/   )######_
	{ ######/  \###| |-__   |#| |_|\/|_|/####\ \| |#\  || |/ ____ \/ /###/ _ \##}
	 \#####/ /\ \##|  |##-__|#|_|#######@@###d@_#_|##\_||_|_/####\_| |##( (#\/#/
	  \###/ ____ \##| |#################Y@b_@@#####################\  )##\ \##/
	   \##_/####\_#######oooOOOOODROOOOOb@@@@dOOGUZOOOOOoooooo#########'\/ /#/
	   \#############OOOOO~~~~~        _-\  /-_       ~~~~~~~OOOOOoo###\__/##/
	    \########OOP~   _              (__ O _/                 ~~OOOOO#####/
	     \#dOOOOOO_    | Y~_            (__d__)                   _OOOOOOb#/
	      OO(###OOO_   | r_|    ____oood@@@@@@@@oooo@F           _OOO###)OO
	       ~~O###OOOo   Y ||   _Y@@@@@@P   ~V~  Y@@@@F          o000###O~~
	          ~`##*OOb  | r_\_/ _)@@@@@          @@@@f  __     _--*##'~
	            `##*OOb_ \  `_ / ~@@@@@    a a   @@@@  (_ `---'  ~~)'
	             ~###OOOo `-__Y)   @@@@b   @_@  d@@@     lv'~   ~~#~
	               `##*OOb   (_@b   Y@~ T  (_) T   )   d@Uknnn###'
	                ~###OOOb    Yb_  (  |\    /|  / od@P bdOO###~
	                  `###OOOb   Y@b  `-| `--'_'_d@@@P dOOO###'
	                   ~####OOOb   Y@b  \\nn //@@@@P dOOO####~
	                     ~####OOOb  ~@@@@\`-'/@@@P~dOOO####~
	                       ~####OOOb  Y@@@@@@@@@~dOOO####~
	                         ~####*OObo@@@@@@@odOO*####~
	                           ~~####*OOOb|dOOO*####~~
	                              ~~#####*|*#####~~
	                                  ~~#####~~                       
	                                      ~     Sean Gugler

And to test it:

	 ~/Programming/brainfuck  $ beef hidden.bf 
	Hello World!

Now that is some truly unreadable code. As an extension, it would be good to spread the BF chars throughout the piece rather than sticking them all at the beginning.
