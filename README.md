WordNet-Dictionary-in-JSON
==============
WordNet dictionary in JSON format for easy dictionary entry query.

# What does this Project Offer?
Princeton's WordNet® is a semantic network with a byproduct as a dictionary. Its core is synset (a set of synonyms) rather than words, and mainly designed for computers to process NLP-related tasks.

Querying a word in the original WordNet database requires some effort to take pieces here and there and weave them together.

These JSON files are the weaved results. They are organized by words, not synsets anymore. You can easily query a word, get its synonyms, antonyms, meanings and examples etc. As a price to pay, easy maneuvering between synsets is lost, which was designed mainly for machines. Human usually follows words, not synsets. So from this perspective, the loss is somewhat desired.

These JSON are split into multiple files by their first letter. Any word starting with a non-alphabetic character is placed into misc.json.

This project is based on WordNet 3.1 database. 

# JSON Structure
```
"ambulatory": {
        "word": "ambulatory",
        "pos_order": [
            "adjective",
            "noun"
        ],
        "meanings": [
            {
                "synset_offset": "02626762",
                "sense_number": 1,
                "part_of_speech": "adjective",
                "def": "relating to or adapted for walking",
                "example": [
                    "an ambulatory corridor"
                ],
                "wordnet_ptrs": {
                    "pertainym_pertains_to_noun": [
                        "ambulation"
                    ],
                    "derivationally_related_form": [
                        "ambulation"
                    ]
                }
            },
            {
                "synset_offset": "01527104",
                "sense_number": 2,
                "part_of_speech": "adjective",
                "def": "able to walk about",
                "example": [
                    "the patient is ambulatory"
                ],
                "synonyms": [
                    "ambulant"
                ],
                "wordnet_ptrs": {
                    "similar_to": [
                        "mobile"
                    ],
                    "derivationally_related_form": [
                        "ambulate"
                    ]
                }
            },
            {
                "synset_offset": "02703984",
                "sense_number": 1,
                "part_of_speech": "noun",
                "def": "a covered walkway (as in a cloister)",
                "example": [
                    "it has an ambulatory and seven chapels"
                ],
                "wordnet_ptrs": {
                    "hypernym": [
                        "walk",
                        "walkway",
                        "paseo"
                    ]
                }
            }
        ]
    },
```
Keys are words. 

The field "word" merely echos the key. 

"pos_order" gives the order of parts of speech when a word has multiple parts of speech. 

"meanings" is an array ordered by what 'pos_order' dictates; within each part of speech, sense_number is used to sort. "def" is the word's definition. It's also the definition for all the other words in the same synset. This is what naturally WordNet offers and you'll see many words have the same definition for their senses (or *meaning* in layman's terms).

"example" contains a list of examples for this word. Examples can be more than one.

"wordnet_ptrs" keeps WordNet's ability to maneuver between words, but not between synsets anymore. All WordNet pointers are parsed into this section, and you may refer to [WordNet definitions of pointers](https://wordnet.princeton.edu/documentation/wninput5wn).

# Lemma vs. Word
WordNet has the concept of lemma. A lemma is in lowercase and spaces between words inside a phrase are replaced by underscores. For example, take off's lemma is take_off. Another example is that Bush and bush are two words, but with the same lemma - bush.

Our JSON files do not have the concept of lemma. Bush and bush are two different keys representing two different words.

There are a few hundred lemmas in WordNet containing multiple words. Part of the reason that we do not have it in JSON is because MySQL and other databases can handle it natively. You may port this JSON file to MySQL, and set *collation* of the column containing key to case insensitive, such as utf8mb4_general_ci.

It's also super easy to convert those JSON files to add another layer of lemma to contain words if you prefer that way.

# Two More things you Need to be Aware
1. The original WordNet synset includes examples for all its members of a synset. Our JSON files only preserve relevant examples for the targeted word. In other words, all examples are distributed to proper word entries. It took us **a lot of effort** to determine which example sentence is for a particular word considering all proper plurals (including irregular and regular) and verb tenses. This is also the hardest part if you would like to convert to JSON files from WordNet from scratch.

2. WordNet orders its PoS (part of speech) most likely to be noun, verb, adj and adv. "pos_order" is what we introduced to rearrange to pump more common PoS up to the front. The orders in JSON files observe "pos_order" field. But, it's your discretion to follow WordNet's original order or the order specified in "pos_order". 

# Alternative REST API call
You may download these JSON files, use them as they are and follow the license requirement. Alternatively, [Dictionary.video](https://dictionary.video) provides a REST API you can call. You'll need to contact us at admin@dictionary.video to get an API key.
```
curl https://dictionary.video/api/wordnet/bush?key=REPLACE_WITH_YOUR_KEY

curl https://dictionary.video/api/wordnet/take%20off?key=REPLACE_WITH_YOUR_KEY

curl https://dictionary.video/api/wordnet/take_off?key=REPLACE_WITH_YOUR_KEY
```
The API call returns a list of words, for example, [Bush, bush] for the query of *bush*. Effectively, this is a lemma query instead of a word query. Every word contains the dictionary format we explained above.

# LICENSE
WordNet-Dictionary-in-JSON by [Dictionary.video](https://dictionary.video) is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License. Please follow the license requirement to acknowledge [Dictionary.video-about](https://dictionary.video/about.html) in any proper manner.

Please also refer to [WordNet License](https://wordnet.princeton.edu/license-and-commercial-use) carefully and follow its requirement. It's also copied below.

> WordNet Release 3.0 This software and database is being provided to you, the LICENSEE, by Princeton University under the following license. By obtaining, using and/or copying this software and database, you agree that you have read, understood, and will comply with these terms and conditions.: Permission to use, copy, modify and distribute this software and database and its documentation for any purpose and without fee or royalty is hereby granted, provided that you agree to comply with the following copyright notice and statements, including the disclaimer, and that the same appear on ALL copies of the software, database and documentation, including modifications that you make for internal use or for distribution. WordNet 3.0 Copyright 2006 by Princeton University. All rights reserved. THIS SOFTWARE AND DATABASE IS PROVIDED "AS IS" AND PRINCETON UNIVERSITY MAKES NO REPRESENTATIONS OR WARRANTIES, EXPRESS OR IMPLIED. BY WAY OF EXAMPLE, BUT NOT LIMITATION, PRINCETON UNIVERSITY MAKES NO REPRESENTATIONS OR WARRANTIES OF MERCHANT- ABILITY OR FITNESS FOR ANY PARTICULAR PURPOSE OR THAT THE USE OF THE LICENSED SOFTWARE, DATABASE OR DOCUMENTATION WILL NOT INFRINGE ANY THIRD PARTY PATENTS, COPYRIGHTS, TRADEMARKS OR OTHER RIGHTS. The name of Princeton University or Princeton may not be used in advertising or publicity pertaining to distribution of the software and/or database. Title to copyright in this software, database and any associated documentation shall at all times remain with Princeton University and LICENSEE agrees to preserve same.