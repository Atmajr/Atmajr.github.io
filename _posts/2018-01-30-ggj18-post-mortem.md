### Education and Motivation through Jamming

This past weekend, January 26th, 2018, was the date of the most recent Global Game Jam, an event where creators and teams worldwide come together and try to build game to fit a previously-unknown theme within a scant 48 hours. Last year's GGJ was my first one (though my fourth game jam overall) and I found it invaluable. The theme that year - "Waves" - led us to creating a pirate-themed adventure game. One of the diversifiers that year - extra challenges you can add to your game to help it stand out - was accessibility for the differently-abled. Because of this, we chose to build a voice-controlled game for the Amazon Echo, to continue to fit with the theme; sound waves, water waves. Building something unique on hardware I'd never worked with and trying to get it running within so short a time was both exhausting and exhilarating, so I jumped at the chance to participate again.

### From Theme to Concept

Huddled together in a small room in the Microsoft building at 6pm Friday, several hundred amateur game designers waited with rapt attention as the sponsoring organizations went through their lectures, all focusing on one thing - what would be the theme announced this year? See, while you can come into the game jam with some prior work or an idea ahead of time, that's very much defeating the point. The idea is to let the theme drive you and let your creativity run wild. It's only 48 hours of your life - you don't have to worry if there's a market for your game, if it will sell, or even if it will be accepted to an online marketplace. You can let your creativity run truly wild.

Finally, at 8pm, the theme was announced - "Tranmission." A broad theme, to be sure, as they seem to always be for GGJ. Instantly, our shared discord and slack lit up with concepts and ideas. Already having a team I was comfortable with, we fled the room as ice-breakers and team matchups started. By the time we'd returned to the apartment we were ready to start whiteboarding. Within about an hour, we had our theme nailed down. We would make a rhythm game where you played as a white blood cell destroying infections in the blood to the beat of the music, which would also be the "patient's" beating hard. Disease transmission, sound transmission. It seemed like a solid concept that fit the theme and, additionally, I thought it would be easier than some of our previous concepts. After all, rhythm games don't have gravity, collision detection, or a physics engine. Notes move from one side of the screen to another, and if you press the button at the right time, you score points. How hard could it be?

### Immediate Difficulties

As soon as I loaded Unity - my engine of choice for this project - and started coding, I realized my mistake. Syncing visual beats to music is really, *really* complicated. The computer doesn't really have a concept of a 'beat', and in Unity, everything happens in frames. What happens when the beat of you song happens between frames? What happens if the framerate is worse on a different piece of hardware? In something like a platformer, it barely matters if your sounds are off by a few milliseconds, but in a precision environment like a rhythm game, timing is critical. The player cannot be made to feel like the timing in the game is 'off' or the game will not be fun. Furthermore, even if you do somehow get things in sync, you cannot just have the note instantly appear. The player needs time to react, so the game must anticipate the beat, instantiate the object, and have it land in scoring position on beat instead. Having only 48 hours to solve this issue, I immediately turned to the first tool in the kit of every dev who's have been under deadline.

### Searching the Internet for Other Approaches

Lucky for me, there were [several](https://youtu.be/kyp3Ks5a6to) [different](http://ludumdare.com/compo/2014/09/09/an-ld48-rhythm-game-part-2/) [posts](http://blog.phantombadger.com/2016/05/23/how-to-create-unity-rhythm-game-part-1-parsing-the-sm-file/) on solving problems just like this one. Unfortunately, we tried these all with varying levels of success. The one that eventually proved the most useful to me was Yu Chao's [devlog](https://www.gamasutra.com/blogs/YuChao/20170316/293814/Music_Syncing_in_Rhythm_Games.php) detailing his approach to rhythm syncing in his game Boots-Cut. This led me to some interesting formulas for converting song tempo to "beats", and finding the nearest "beat" from `AudioSettings.dspTime` to garner the most accurate info. Additionally, although we had to alter the math significantly, his utilization of interpolation for movement proved vital. I just wish we had stumbled on it earlier.

### Failure to Launch

Sadly, this marks the first of my five game jams where we failed to submit a playable product by deadline (3pm on the Sunday following the start). The game is not completely broken. It compiles to Android as desired, the scene transitions all work, and the beat pattern does in fact spawn viruses as we'd like and move them to their desired scoring positions. The issues are in scoring, which is not implemented at all, and beat synchronization, which requires further testing and may need a total overhaul. In this state, we did not feel we could submit a product. That said, I do hope to put at least a little more time into it to get the game to a state where it could at least be called a demo and I can feel proud hosting it on my [itch.io](https://atmajr.itch.io/) page along with my other game jam projects.

<iframe width="560" height="315" src="https://www.youtube.com/embed/krCCl6Qnx9E" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### Future Progress

So, what is the state of the game - which we dubbed Sonic HemoFyte - and what can be said for the future of the project?

I would class the state of the game as pre-pre-alpha, and in-progress. As for what needs to be done, the answer is quite a lot actually, and much of it depends on setting up some sort of unit testing. Here are some things I think we might need to do to complete this project:

- Revisit and refactor core gameplay entirely. We may need to consider changing this game from a rhythm game to a tap-based rail shooter or something. The good news is, I think there could be a market for a semi-educational, kid-friendly game of this type.
- If we keep the rhythm aspect, we may need to change the overall design. I'm not convinced a rhythm game in this perspective and with these visuals can work. Generally, notes in rhythm games are a universal and simple shape and animations take place in the background. Perhaps moving the graphics into a position as more "flash" instead of being the interactable UI would help. It may also help to have the objects moving linearly across the X or Y axis instead of flying at you.
- Calibration. Even if we get the beat syncing properly on one device, there is no guarantee that other visual, audio or input devices will not introduce new lag that we can't anticipate. [This reddit post](https://www.reddit.com/r/gamedev/comments/13y26t/how_do_rhythm_games_stay_in_sync_with_the_music/) includes some insight into how to build out a calibration system similar to the one in guitar hero to alleviate this issue.
- Adding more songs. One isn't enough, obviously. I'd also like to add music by some other artists, including writing some tracks of my own now that I have more time.
- Bugfixing. Always.

### Wrap Up

Even though I can count this among one of our least successful jams, I still can't say I'm sorry I did it or that I wish we had chosen another game type. I've learned so much about Unity, C# and even just coding in general in such a short time, that I think the frustration, stress and trauma were probably worth it. Who knows? Maybe will finish and release the game some day. And if not, there's [always another jam](https://itch.io/jams) on the horizon.

