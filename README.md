# gogen
An open language learning platform, built around efficiency and extensibility.

At this time, gogen is under development, and is not ready to be distributed. As soon as it is in a state where it can actually be used, this page will be updated with install instructions.

## Why do we need gogen?
Language learning is a long, complicated task, at which many ultimately fail. It can be expensive, between subscriptions, books, classes and tutors. It can be tedius, requiring repetition, such as hundreds of flash card reviews a day. It can cause uncertainty, as new methods and opinions force us to re-evaluate our strategies and processes. Finally, it can be unrewarding, when one tries to consume their target language in some way, such as watching a show or reading a book, only to find that months or years of learning haven't proved enough.

On and off I've spent more than 10 years trying to learn Japanese. There is a huge community online, full of tips and tricks, free resources, and everything that you need to (in theory) become fluent. Eventually, I become innundated with an impossible amount of anki (flashcard) reviews, or hit a plateau where I stop feeling the progress, or become unable to transition into more disciplined phases, such as trying to slowly read a book, or consume pure native content. Each time I restart, I research and build a plan based on the current advice I can find. I'll start a new flashcard deck, and have to skip over the hundreds of words I remember.

gogen aims to solve this by becoming a full suite of learning. It aims to teach you, keep track of what you know, help you push words into long term memory, and make immersion easier.

## How does it work?
In language learning, there are three concepts which are recommended almost universally:

1. Stephen Krashen's theory of comprehensible input: The idea that all language is learned by "filling in the gaps". When you have enough context that you can understand a word that you've never heard before, you begin to understand the new word, how it is used, and how it fits into the sentance.
2. SRS (Spaced Repetition Systems): The idea that long term memory is built when your brain learns something is important to you. By learning something, and then recalling that information just before it is pushed out of your short term memory, your brain will hold on to the information for longer. Repeating this over and over can be the quickest way to learn vocabulary, and a range of other concepts as well. The big example here would be Anki.
3. Immersion: The more you "immerse" yourself in a target language, the more you learn. While the best way to immerse is to physically be in a country where you are surrounded by your target language, there are many ways you can immerse at home, using media and books.

gogen aims to combine all three of these into one learning system. This learning system will accurately keep track of what you know and what you don't. Using that information, along with the plugin system, it will be possible to further enhance learning, such as having TV shows suggested automatically when you can understand a certain percentage of it.

### The learning system
The basic idea is this:

Every day, you will do a review. gogen will determine which words need to be reveiwed, and which words can be learned. You will be presented with a sentance containing one new word, and several "known" words. If you don't understand a word, you can tap it to learn more. This can have levels, such as starting with Japanese characters, showing their phonetic spelling, and finally showing the definition. Once you understand the sentance, you can click "next". If you don't reveal a word, gogen assumes you understood it, and considers you to understand that word a little better. If you do reveal a word, gogen lowers your comprehension score for that. There will also be an "explain" button, which will break down the grammar, and gogen will keep track of those grammar points as well.

Since gogen keeps track of which words you know, and how well you know them, it can suggest shows, movies, books and other content based on how much of the wording you will understand. Additionally, googen will be able to keep track of what content you consume, and automatically add sentances to the pool of review sentances. 

This way, you can spend less time reviewing each day. By switching up the example sentances, gogen can ensure you didn't just memorize the full sentance. 

## Downsides
- At first, gogen will be developed specifically for Japanese. While I aim to keep it as open as possible, so other languages can be added, I can't commmit the time required to make every language work
- gogen will require self-hosting, as I do not have the funds to host it for everybody who might want to use it. I understand that limits it's access for less technical people, or people without the means of hosting it themselves, and if another route becomes possible I will explore it.
- Because I have some of the basics down for Japanese, this app will be biased for intermediate learners. It is more difficult to produce an "i+1" sentance when the user doesn't know any words.

## AI Policy
I am a software developer by trade, and as such I will do my best to keep this in a clean, secure and maintainable state. However, for me, a project of this scale and claiber is not possible without AI development. 