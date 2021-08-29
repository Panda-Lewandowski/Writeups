# Message from Foma

The first two step are really easy, my friend.
Yes, of couse, you know that binary hex encoding.

```
01100100 00110000 01100001 00110111 01100100 00110000 00111001 00110100 01100100 00110000 01100001 00110110 00110101 01100001 01100100 00110000 00111001 00110100 01100100 00110010 01100001 01100001 01100100 00110000 00111001 00110011 01100100 00110011 01100010 01100001 00110111 01100010 01100100 00110000 01100001 00110100 01100100 00110011 00111000 00111001 00110101 01100110 01100100 00110010 00111001 00110000 01100100 00110011 00111000 00111001 01100100 00110000 01100001 00110100 00110101 00110011 01100100 00110000 01100001 01100100 00110101 01100110 00110101 00110011 01100100 00110000 00111001 00110011 01100100 00110000 01100001 01100110 01100100 00110000 00111001 00110100 01100100 00110000 00111001 00111000 00110100 00110111 01100100 00110000 01100001 01100100 00110101 01100110 01100100 00110000 01100001 01100110 01100100 00110000 00111000 01100110 00110101 00110011 00110101 00110011 01100100 00110000 00111000 00110111 01100100 00110000 00111001 00110100 01100100 00110000 00111001 00111001 00110101 00110011 00110111 01100100
```

```
d0a7d094d0a65ad094d2aad093d3ba7bd0a4d3895fd290d389d0a453d0ad5f53d093d0afd094d09847d0ad5fd0afd08f5353d087d094d099537d
```

```
ЧДЦZДҪГӺ{ФӉ_ҐӉФSЭ_SГЯДИGЭ_ЯЏSSЇДЙS}
```

But what is that? It is a flag? No! Is it cyrillic? What a strange cyrillic!
Foma's real name is Ащьф Лштшфум and on August 21th he had a birthday. So he wanted to prank us and sent message with a Fake Russian by secret channel!

After googling we clash with [this site](https://www.spammimic.com/decoderus.cgi). And it didn't help!

Another good variant is [this](https://jkirchartz.com/demos/fake_russian_generator.html), but it doesn't have decoder. Also you can notice that it's unambiguous encoding. What should we do next?

We know that the format of the flag is `YauzaCTF{}`. We can compare the output of this site with our string and it have something similar. 

The easiest way is to find [a repo](https://github.com/JKirchartz/demos/blob/gh-pages/fake_russian_generator.html) with this encoder and handly decode a message.

```js
alpha: {
        'A': ['Д'],
        'B': ['Б', 'Ъ', 'Ь'],
        'C': ['Ҫ'],
        'E': ['Ԑ', 'Є', 'Э'],
        'F': ['Ӻ', 'Ғ'],
        'H': ['Њ', 'Ҥ', 'Ӊ', 'Ң'],
        'I': ['Ї'],
        'K': ['Қ', 'Ҡ', 'Ҝ', 'Ԟ'],
        'M': ['Ԡ'],
        'N': ['И', 'Ѝ', 'Й'],
        'O': ['Ф'],
        'R': ['Я'],
        'T': ['Г', 'Ґ', 'Ҭ'],
        'U': ['Ц','Џ'],
        'W': ['Ш', 'Щ'],
        'X': ['Ӿ', 'Ҳ', 'Ӽ', 'Ж'],
        'Y': ['Ч', 'Ұ']
        },
```

**Flag:** `YauzaCTF{OH_THOSE_STRANGE_RUSSIANS} / YAUZACTF{OH_THOSE_STRANGE_RUSSIANS}`
