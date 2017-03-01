---
layout: post
title: Connect Four
description: CLI Connect Four Game
published: true
img: /img/bash.jpg
---
<h2>
<a href="https://github.com/Thomascountz/odin_projects/tree/master/connectfour" target="_blank">Connect Four</a> 
</h2>

<br>
<h1>What is this?</h1>
<br>

<p>A command-line Connect Four game that I wrote while working through <a href="http://www.theodinproject.com" target="none">The Odin Project</a>.</p>

<br>
<h1>What does it do?</h1>
<br>

<p><a href="https://en.wikipedia.org/wiki/Connect_Four" target="none">Connect Four</a> is a board game for two players. The board has a seven rows and six columns. Each player takes turns stacking their colored tokens into the bottom-most position of each of the six columns. The first player to stack four tokens in a row, either horizontally, vertically, or diagonally, wins! This command-line game is exactly that.</p>

<br>
<h1>What's inside?</h1>
<br>

<p>Firstly, writing this game really helped to understand OOP and TDD principles. My file tree looks like this:</p>

{% highlight ruby %}
  .
  ├── connect_four.rb
  ├── lib
  │   ├── board.rb
  │   ├── game.rb
  │   └── player.rb
  └── spec
      ├── board_spec.rb
      ├── game_spec.rb
      ├── player_spec.rb
      └── spec_helper.rb
{% endhighlight %}

<p>As you can probably tell, my focus from early on was with the separation of concerns. Each class was contained with it's own file, and each class had it's own test. Speaking of tests, this project was my first experience using doubles like <code>player1 = double('player')</code> to ensure that one class' tests aren't dependent on a separate class.<br><br>The other very fun part of building this game was writing the <code>Board#win?</code> method. I approached this problem a few different ways. The first thing to note is that I used a nested array to represent my board:</p>

{% highlight ruby %}
[['.', '.', '.', '.', '.', '.', '.'],
 ['.', '.', '.', '.', '.', '.', '.'],
 ['.', '.', '.', '.', '.', '.', '.'],
 ['.', '.', '.', '.', '.', '.', '.'],
 ['.', '.', '.', '.', '.', '.', '.'],
 ['O', '.', '.', '.', '.', '.', '.']]
{% endhighlight %}


<p>So therefore, the position where the <code>'O'</code> is located could be indexed as <code>Board[5][0]</code> (The first element in the fifth array). When it came time to write a <code>Board#win</code> method, I first approached it recursively. I wanted the method to return true or false by following a "flood-fill" methodology. The idea was that when a token was played, those "coordinates" would be passed to <code>Board#win?</code>. The base cases would be to <code>return if count == 4</code>, meaning that four tokens were found in sequence, and to <code>return if !row.between?(0, 5) && !column.between?(0, 6)</code>, meaning that the method was now testing outside of the board's range.<br><br>If the current position on the board was equal to the token, the method would check each position in each direction to see if there was a matching token, if so, it would move in that direction and <code>count += 1</code> until either <code>count == 4</code> or a matching token was not found. In theory, this would work, and I'm still excited by the prospect of implementing this algorithm, as I think it is suitable, but instead, I settled on a non-recursive strategy:</p>

{% highlight ruby %}
def horizontal_win?(args = {})
  token   = args.fetch(:token, nil)
  row     = args.fetch(:row, nil)
  column  = args.fetch(:column, nil)

  count = 0

  4.times do |i|
    count += 1 if column + i <= 6 && @play_area[row][column + i] == token
    count += 1 if column - i >= 0 && @play_area[row][column - i] == token
    return true if count == 5
  end

  false
end
{% endhighlight %}

<p>This method was written to check for horizontal wins and similar methods were written to check for diagonal and vertical wins as well. Firstly, this code accepts an <code>args</code> array to take the pressure of the method caller to keep track of so many representational numbers. Secondly, the code iterates four times and simply adds one to <count>count</count> if token in either direction matches the token that was just played.<br><br>I had some weird edge case errors with the code initially, for example, when <code>column - i</code> would equal a negative number, the method would then be checking the end of the array, and if their happened to be a matching token on the opposite side, a win would be returned. Luckily testing helped with this!</p>

{% highlight ruby %}
  it 'returns false' do
    board.instance_variable_set(:@play_area,
                                [['.', '.', '.', '.', '.', '.', '.'],
                                 ['.', '.', '.', '.', '.', '.', '.'],
                                 ['.', '.', '.', '.', '.', '.', '.'],
                                 ['.', '.', '.', '.', '.', '.', '.'],
                                 ['.', '.', '.', '.', '.', '.', '.'],
                                 ['O', '.', '.', '.', 'O', 'O', 'O']])
    expect(board.win?(token: 'O', row: 4, column: 0)).to be false
  end
{% endhighlight %}

<p>This test originally failed.<br><br> Overall, this was one of my favorite projects to work on. From end to end, I felt very confident, and now I'm excited to go back and refactor!</p>

<br>
<h1>Where do we go from here?</h1>
<br>

<p>Refactoring and playtime! With this code and tests in hand, I can now try to implement a recursive method to check for winning patterns, I can try to implement a <a href="https://chessprogramming.wikispaces.com/Bitboards">bitboard</a>, and I can deploy a frontend so that I can play online with friends!</p>

<br>
<h1>What did I learn?</h1>
<br>
<p>Well if I haven't gone through it all already, I've learned TDD with RSpec, OOP and separation of concerns, and to have fun trying new things!</p> 