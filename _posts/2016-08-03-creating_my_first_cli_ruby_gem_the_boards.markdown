---
layout: post
title:  "Creating my First CLI Ruby Gem. The Boards"
date:   2016-08-03 15:24:27 -0400
---


Stuck. Confused. Terrified. Why? Because there I was with at a blank terminal, a blank text editor, a blank stare, and too many ideas flowing through my head all at once. Somehow I was supposed to take all the knowledge that's been crammed into my head and output software that works. This experience reminds me of the first time I composed music. Blank canvas with the recorder recording for hours on end but nothing to show for it.

Similar situation here, here I was about 2 hours later and still nothing. I hadn't been this paralyzed since the first time I asked a girl on a date...yeah no joke. So with nothing on my canvas and a million ideas in my mind fighting for the top spot, I put everything down and went to the studio to do what I do best. Make dope hiphop beats over symphonic melodies, bangin drums, and super deep heavy hitting bass. Well all that led to an idea sprouting in my mind natrually for the CLI gem. My thoughts began to scream at me, "**DO WHAT YOU KNOW! YOU KNOW MUSIC! GO MAKE SOMETHING DOPE!**". I couldn't wait to get back home to try out my idea I had finally landed on. Thus born is THE_BOARDS.

The_Boards is a CLI ruby gem that pulls in data directly from the billboard charts to display a list of the hottest 100 songs of the week. I figured it would be cool if other apps used the data in their apps or websites to always have access to the hottest songs curated by the billboard charts.
<iframe width="420" height="315" src="https://www.youtube.com/embed/BSKO9q4qslE" frameborder="0" allowfullscreen></iframe>

> "DO WHAT YOU KNOW! YOU KNOW MUSIC! GO MAKE SOMETHING DOPE!"

### Installation and Usage
This cli gem is simple but effective and has one major task. Just provide dope songs weekly. The gem can be found on [RubyGems](https://rubygems.org/gems/the_boards) or installed though your terminal or gemfile with the following:

> Add this line to your application's Gemfile:
> 
> `gem 'the_boards'`
> 
> And then execute:
> 
> `$ bundle`
> 
> Or install it yourself as:
> 
> `$ gem install the_boards`

The cli gem includes an executable, so once installed you can run the program by typing `the-boards` in your terminal. You should then get the following output: (You may see different songs listed as this list is updated weekly)

```
[13:30:37] ~
// ♥ the-boards
Today's Hottest Songs:
1. Cheap Thrills
2. Cold Water
3. One Dance
...
...
```
Continue listing songs untill 100
```
...
...
98. Light It Up
99. Middle Of A Memory
100. Castaway
Enter the number of the song you'd like to see or type list to see the list again or type exit:

Not sure what you want, type list or exit.
```

Next you can select a song for more information by typing in the number. For example typing 10 gives me:

```
10
'Panda' by Desiigner is currently #10 on the charts.
Last Week: 9
Enter the number of the song you'd like to see or type list to see the list again or type exit:
```

You then can select another song by number, display the list again by typing `list` or exit by typing `exit`.

Upon exiting, you'll be invited to come back next week for a new updated list.
```
exit
See you next week for more songs!!!
[13:50:09] ~
// ♥ 
```


### In Conclusion
Hope you found this blog to be informative and entertaining. This is my first ruby cli gem. Feel free to use it in your next project. If you find any bugs or have suggestions that could make it even better, shoot me an email at branden@thisistydell.com or find me on [GitHub](http://www.github.com/thisistydell) - www.github.com/thisistydell

Here's an example on [YouTube](http://https://www.youtube.com/watch?v=BSKO9q4qslE). https://www.youtube.com/watch?v=BSKO9q4qslE 
You can get this gem on [RubyGems](https://rubygems.org/gems/the_boards)

```exit```




