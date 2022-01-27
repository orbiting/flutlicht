# Flutlicht

## Preface

There are multiple indices available but only few are used to for search feature: comment-index, document-index and user-index.

Each folder in this repo contains index mapping, query and doc samples as JSON. Some doc properties may have been redacted (`[[…]]`) but tend to not affect search results.

We search indices all at once (see ./query-sample-multiple-indices.json) or per "type": comment, document or user.

Republik runs a ElasticSearch 6.8.x cluster on elastic.co

## Indices

### comment-index

Contains user comments ("Dialogbeiträge", Kommentarspalte)

### document-index

Contains articles

### user-index

Contains users ("Profile")

## Words

./words-sort-uniq.txt contains a rough but sorted list of uniq words used in documents published as of Januar 27, 2022.

### Processing snippet

```js
const { meta, contentString } = hit._source

const words = contentString
  .concat(` ${meta.title} ${meta.creditsString}`)
  .replace(/[.:;,\n]/gi, ' ')
  .split(' ')
  .map((s) => s.trim().toLowerCase().replace(/[^\p{L}\-]/giu, ''))
  .filter(Boolean)

words.map(word => console.log(word))
```

```sh
script.js > words.txt
words.txt | sort > words-sort.txt
words-sort.txt | uniq > words-sort-uniq.txt
```