Review System

This document outlines the review system of gogen. The review system tries to optimize traditional SRS by intelligently choosing sentances rather than single words.

SRS style reviews have multiple contexts. The first is the "learning" phase, where you are presented with a new word. The second is the "review" phase. 
With SRS, the idea is that by recalling a word just before your brain "forgets" it, your brain will keep it for longer, and by repeating this process you can fully learn words.

When actually learning a language, you won't be waiting months between using words, since you will be consuming that language and seeing the word in multiple contexts.

Therefore, what this system aims to do is deliver review sentances which are designed to pack in as much learning and review as possible, without taking hours. 

New words:
    New words are best learned using the "n+1" method. This is according to Stephen Krashen's theory of comprehensible input https://www.youtube.com/watch?v=9stabebgg14
    Essentially, If there is one unknown element, as long as sufficient context exists, you should be able to understand the sentance, and learn the new word.

Review words:
    If we are doing n+1 sentances for new words, naturally we will be reviewing the "n" words, or the already known words. When this happens, we can mark off the word as reviewed.
    However, different words hold different weight in a sentance. In order to properly measure the review count, or the time until next review, we need to consider how important 
    the word being reviewed was. When inputting, or "mining" sentances for review, gogen will score each word. Then, when one of the words is a "review", we can treat that as a 
    fraction of a review, based on the score we give the word. If a word is 100% required, then the word is fully reviewed. If it is 50% required, it is 50% reviewed

Scoring:
    With SRS, such as anki, users self-report how easy the word was to recall. Several sources will suggest that a user only choose "I know the word" or "I don't know the word". 
    With gogen, when seeing a sentance, you can choose "next" if you feel you understand the sentance fully. If you don't recognize a word, just tap it to see its definition*. 
    If you don't know the grammar, click "explain grammar". Once you have all you need to understand the sentance, click "next". No need to feel bad, and no pressure to "lie" or misreport.

    When you choose to reveal a word, gogen keeps that in mind, and lowers your comprehension level for that word. This way, you will be more likely to see the word again sooner, and 
    hopefully that keeps it in your memory. If you keep revealing the word, gogen no longer considers it a word you know, and it will attempt to teach it to you again.

    *Tap to reveal is going to be different for each language. For Japanese, we can start with showing furigana, before showing the english definition. Similarly, grammar points
    in Japanese can first be deconstructed, using a known system such as Cure Dolly's, before showing a definition.

Comprehension:
    In order to automate n+1 learning, gogen needs to understand what words you know. With other SRS, if you have an example sentance, you might not actually learn a word, but rather the
    surrounding sentance. Seeing the word again in new context will throw you off. To combat this, as well as deliver appropriate review words, gogen does not reuse example sentances every time.
    Instead, it continually mines new sentances, and gives you an appropriate sentance each time.