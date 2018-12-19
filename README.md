
# JP Yomi Ezzat 

This simple code takes data from [JMdict](https://www.edrdg.org/jmdict/j_jmdict.html), [KANJIDIC](http://www.edrdg.org/wiki/index.php/KANJIDIC_Project), and [Chinese characters decomposition](https://commons.wikimedia.org/wiki/Commons:Chinese_characters_decomposition), combines them, and generetes a CSV table that can be imported in [Anki](https://apps.ankiweb.net).

## FAQ

### Which items are included?

The contents are :
- Words from **JMdict** that contains has frequency tags and contains kanji,
- Words from **JMdict** that don't have frequency tags but appear in ManyThings examples, Wanikani vocabs, Core 6K, Core 10K, and Tanos JLPT vocabs, 
- Kanji letters from **KANJIDIC** from kanken 10 to kanken 1.

### What is JMdict frequency tag

Taken from https://www.edrdg.org/jmdict/edict_doc.html

> Word Priority Marking
> 
> The ke_pri and equivalent re_pri fields in the JMdict file are provided to record information about the relative commonness or priority of the entry, and consist of codes indicating the word appears in various references which can be taken as an indication of the frequency with which the word is used. This field is intended for use either by applications which want to concentrate on entries of a particular priority, or to generate subset files. The current values in this field are:
> 
> - news1/2: appears in the "wordfreq" file compiled by Alexandre Girardi from the Mainichi Shimbun. (See the Monash ftp archive for a copy.) Words in the first 12,000 in that file are marked "news1" and words in the second 12,000 are marked "news2".
> ichi1/2: appears in the "Ichimango goi bunruishuu", Senmon Kyouiku Publishing, Tokyo, 1998. (The entries marked "ichi2" were demoted from ichi1 because they were observed to have low frequencies in the WWW and newspapers.)
> - spec1 and spec2: a small number of words use this marker when they are detected as being common, but are not included in other lists.
> - gai1/2: common loanwords, also based on the wordfreq file.
> nfxx: this is an indicator of frequency-of-use ranking in the wordfreq file. "xx" is the number of the set of 500 words in which the entry can be found, with "01" assigned to the first 500, "02" to the second, and so on.
> 
> Entries with news1, ichi1, spec1/2 and gai1 values are marked with a "(P)" in the EDICT and EDICT2 files.

### 30k+ notes are too much, how do I reduce the items?

Yeah, I don't recommend to learn all of them. You can always suspend and unsuspend using the tags and sources field.

### I also use wanikani, how do I remove the items to avoid overlapping? 

All items are tagged with their sources, so you can use `"deck:JP Yomi Ezzat" sources:*wk*` filter in anki and suspend the items.

### I don't want to include Kanji Kentei level 1.

Filter the deck `"deck:JP Yomi Ezzat" (tag:kanji_jitenon_kanken1jyun or tag:kanji_jitenon_kanken1 or tag:kanji_jitenon_kanken1alt)` and suspend them.

### Is there audio in this deck? 

No :(, I can't find open source audio sources that I can use here.

### Is there direct link to the Anki deck?

~ I'll upload it soon...

## Create the CSV deck

Put the sources in the source folder
```
# Source: http://www.edrdg.org/kanjidic/kanjidic2.xml.gz
source/kanjidic2.xml

# Source: http://ftp.monash.edu/pub/nihongo/JMdict_e.gz
source/JMdict_e

# Source: https://commons.wikimedia.org/wiki/User:Artsakenos/CCD-TSV
source/CCD.csv
```

and then

```
mkdir -p dist
csvtojson --delimiter="\t" --noheader="true" --headers="[\"empty\",\"char\",\"strokes\",\"composition_kind\",\"first\",\"first_strokes\",\"first_verification\",\"second\",\"second_strokes\",\"second_verification\",\"cangjie_coding\",\"radical\"]" ./source/CCD.csv > ./dist/CCD.json
xml2json ./source/kanjidic2.xml ./dist/kanjidic2.json
xml2json ./source/JMdict_e ./dist/JMdict.json
node --max-old-space-size=4096 code.js
node --max-old-space-size=4096 makecsv.js
```

## Acknowledgement

- JMdict https://www.edrdg.org/jmdict/j_jmdict.html
- KANJIDIC http://www.edrdg.org/wiki/index.php/KANJIDIC_Project
- Chinese characters decomposition https://commons.wikimedia.org/wiki/Commons:Chinese_characters_decomposition
- Japanese Core 6000 (Core 6K) https://iknow.jp/content/japanese
- Japanese Sensei (Core 10K) http://en.colezhu.com/jsensei/
- Jitenon https://kanji.jitenon.jp
- Kanshudo Collection https://www.kanshudo.com/collections
- ManyThings Kanji Dictionary http://www.manythings.org/kanji/d/index.html
- Tanos JLPT http://www.tanos.co.uk/jlpt/
- Wanikani https://wanikani.com
