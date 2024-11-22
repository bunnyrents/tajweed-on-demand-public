# tajweed-on-demand-public

Mostly needed to publicly serve the following:

- Quran's fonts.
- Quran Chapters JSON Data.
- Locales Files for both customer & reciter apps.

## Quran's fonts

All fonts have been downloaded from [`https://github.com/quran/quran.com-images/tree/master/res/fonts`](https://github.com/quran/quran.com-images/tree/master/res/fonts).

**NOTE:** We changed the extention to be `.ttf` (instead of `.TTF`) as `.ttf` is a MUST!

## Quran Chapters JSON Data

### How were generated?

All 114 Quran Chapters have been dynamically fetched into jsons files using the following bash command

**NOTE:** You can also use this api `https://api.quran.com/api/v4` in case `https://api.qurancdn.com/api/qdc` was offline (both apis should return the same JSON data)

```bash
    set -B
    for chapterNum in {1..114}; do
      curl -o ./quran-chapters-json-data/chapter_${chapterNum}.json "https://api.qurancdn.com/api/qdc/verses/by_chapter/${chapterNum}?per_page=all&words=true&fields=chapter_id,text_uthmani&word_fields=verse_key,verse_id,page_number,location,text_uthmani,code_v1,code_v2"
    done
```

### The Type/Format of the JSON file:

```ts
type JSONFile = {
  verses: Verse[];
  pagination: any; // Ignore it ^_^
};

type Emptiable<T> = T | null | undefined;

/**
 * The verse that was fetched from the api.
 */
type Verse = {
  id: number;
  verse_number: number;
  verse_key: string;
  hizb_number: number;
  rub_el_hizb_number: number;
  ruku_number: number;
  manzil_number: number;
  sajdah_number: Emptiable<number>;
  chapter_id: number;
  text_uthmani: string;
  page_number: number;
  juz_number: number;
  words: VerseWord[];
};

/**
 * A word for a specific verse that was fetched from api.
 */
type VerseWord = {
  id: number;
  position: number;
  audio_url: Emptiable<string>;
  verse_key: string;
  verse_id: number;
  location: string;
  text_uthmani: string;
  code_v1: string;
  code_v2: string;
  char_type_name: string;
  page_number: number;
  line_number: number;
  text: string;
  translation: {
    text: string;
    language_name: string;
    language_id: number;
  };
  transliteration: {
    text: Emptiable<string>;
    language_name: string;
    language_id: number;
  };
};
```
