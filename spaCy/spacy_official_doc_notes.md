# Notes for Reading spaCy Official Documentation

# Resources

- [Universal PoS Tags (UPOS)](https://universaldependencies.org/docs/u/pos/)
- [OntoNotes Dataset](https://catalog.ldc.upenn.edu/LDC2013T19)
- [Jieba Chinese Segmentation](https://github.com/fxsjy/jieba)
- [PKUSEG Chinese Segmentation](https://github.com/explosion/spacy-pkuseg)

# Loading Models & Languages

- Load models
    - using spacy's loader

        ```python
        import spacy
        nlp = spacy.load('en_core_web_lg')
        ```

    - import models as modules (for larger code base)
        
        ``` python
        import en_core_web_lg
        nlp = en_core_web_lg.load()
        ```

- Multi-language support

    ```python
    # Standard import
    from spacy.lang.xx import MultiLanguage
    nlp = MultiLanguage()

    # With lazy-loading
    nlp = spacy.blank("xx")
    ```

- 3 segmentation options for Chinese: `char`(default), `jieba`, `pkuseg`.

- Pre-trained models can be specified as dependencies in requirement.txt.

# Lingustic Features & Annotations

- Annotations are available as token attributes. Token attributes without suffix `_` are hash values, the ones with suffix `_` are stings. For example, `token.pos_` is can be of values `PROPN`, while `token.pos` is the corresponding hash value. 

## Tokenization
- split by whitespace, and for each substring:
- Check execption rules for each language, e.g. U.K. should not be splitted.
- Check if a prefix, suffix, or infix can be split off.
- Start next iteration on newly split substring.

## Part-of-Speech Tags & Dependencies
- Needs a pre-trained model
- Types: `lemma_`, `pos_`, `tag_`, `dep_`, `shape_`, `is_alpha`, `is_stop`
    - Text: The original word text.
    - Lemma: The base form of the word.
    - POS: The simple UPOS part-of-speech tag.
    - Tag: The detailed part-of-speech tag.
    - Dep: Syntactic dependency, i.e. the relation between tokens.
    - Shape: The word shape – capitalization, punctuation, digits.
    - is alpha: Is the token an alpha character?
    - is stop: Is the token part of a stop list, i.e. the most common words of the language?
- Can be visualized using spaCy's displaCy visualizer
- `spacy.explain("<attribute-name>")` can be used to understand the meaning of the attribute.

## Named Entity
- A "real world object" that was assigned a name, e.g. a person, a place, a book title.
- Accessible via `ents` property (iterable) of a `Doc`
- Common attributes of an `ent`: `ent.text`, `ent.start_char`, `ent.end_char`, `ent.label_`
    - Text: The original entity text.
    - Start: Index of start of entity in the Doc.
    - End: Index of end of entity in the Doc.
    - Label: Entity label, i.e. type.
- Can be visualize using displaCy visualizer.

## Word Vectors & Similarities
- Word vectors are generated using algorithms like word3vec.
- Access token vectors using `token.vector` attribute. `Doc.vector`s and `Span.vector`s are the average of token vectors by default.
- Common attributes related to word vectors: `token.has_vector`, `token.vector_norm`, `token.is_oov`:
    - Text: The original token text.
    - has vector: Does the token have a vector representation?
    - Vector norm: The L2 norm of the token’s vector (the square root of the sum of the values squared)
    - OOV: Out-of-vocabulary
- A larger pre-trained models contains longer list of vocabulary, thus fewer words will have `is_oov` as `True`.
- Similarities (between 0 and 1) can be computed using `similarity()` method of `Doc`, `Span`, `Token`, `Lexeme`.
- Similarities between `Lexeme`s are the same with the corresponding `token`s.
- Since `Doc.vector`s and `Span.vector`s are averages of `Token.vector`s by default, word order does not matter.
- Small pre-trained model (e.g. `en_core_web_sm`) do not contain actual word vectors. Similarities can still be computed if a "small model" is used, but the results are not as good. Use a larger model if accurate similarites are required.


# Pipelines

- A typical pipeline: Text --> Tokenizer (must have) --> Processing Pipeline (Tagger, Parser, Lemmatizer, Ner, ...) --> Doc

- Components
    - `Tokenizer`: Segment text into tokens; Creates `Doc`.
    - `Tagger`: Assign part-of-speech tags; Creates `Token.tag`.
    - `DependencyParser`: Assign dependency labels; Creates `Token.dep`, `Token.head`, `Doc.sents`, `Doc.noun_chunks`.
    - `EntityRecognizer`: Detect and label named entities; Creates `Doc.ents`, `Token.ent_iob`, `Token.ent_type`.
    - `Lemmatizer`: Assign base forms; Creates `Token.lemma`.
    - `TextCategorizer`: Assign document labels; Creates `Doc.cats`.
    - custom components: Assign custom attributes, methods, or properties; Creates `Doc._.xxx`, `Token._.xxx`, `Span._.xxx`.
    - `AttributeRuler`
    - `EntityLinker`
    - `EntityRuler`
    - `Morphologizer`
    - `SentenceRecognizer`
    - `Sentencizer`
    - `Tok2Vec`
    - `TainablePipe`: all component class inherit from this one
    - `Transformer`
    - Other functions

- Pipeline and its components can be specified in the config file like the following:

    ```
    [nlp]
    pipeline = ["tok2vec", "tagger", "parser", "ner"]
    ```

- Components are usually independent so the order does not matter. However, in some cases, they may share some "token-to-vector" components like `Tok2Vec` or `Transformer`.

- Components can be added using `Language.add_pipe` (`nlp.add_pipe()`).

# Architecture

## Data Structure

- Data structure of spaCy ensures a single source of truth and avoids storing multiple copies of the data.
- The `Doc` object owns the sequence of tokens and their annotations, `Span`s and `Token`s are views that point into it.

- Container objects
    - `Doc`
    - `DocBin`: collection of `Doc`s
    - `Example`
    - `Language`
    - `Lexeme`
    - `Span`
    - `SpanGroup`: collection of `Span`s
    - `Token`

## Matcher

- `DependencyMatcher`: Match sequence of tokens based on dependency trees using "Semgrex operators"
- `Matcher`: rule based
- `PhraseMatcher`: based on list of `Doc` (e.g. read from a file)

## Other struture

`Corpus`, `KnowledgeBase`, `Lookups`, `MorphAnalysis`, `Morphology`, `Scorer`, `StringStore`, `Vectors`, `Vocab`

# Vocab, Hashes, and Lexemes

- A `Doc` is consist of `Token`s.
- A `Vocab` is consist of `Lexeme`s.
- A `StringStore` is a 2-way lookup table between texts and hashes.
- `Lexeme` is hashed tokens with **context-independent** information. Following are some common attributes of a `lexeme`:
    - `text`
    - `orth`
    - `shape_`
    - `prefix_`
    - `suffix_`
    - `is_alpha`
    - `is_digit`
    - `is_title`
    - `lang_`
- Words with different capitalizaiton, etc will lead to different `Lexeme` hashes, e.g. Apple v.s. apple.
- As long as the string of a text is the same (from different doc), it shares the same lexeme (hash).
- `Lexeme` can be accessed via `doc.vocab[token.text]` or `token.lex`.

# Serialization (Saving & Loading)

- spaCy supports Python pickle protocol.
- `Language`, `Doc`, `Vocab`, `StringStore` all have four methods: `to_bytes`, `from_bytes`, `to_disk`, `from_disk`.

# Training

- All settings and hyperparameters can be set in `config.cfg`.
- Same config file must be loaded in runtime.

# Language Data

- `lang` module contains all language-specific data. Files in `lang` module:
    - `stop_words.py`, `tokenizer_exceptions.py`, `punctuation.py`, `char_classes.py`, `lex_attrs.py`, `syntax_iterations.py`, `lemmatizer.py`, `spacy-lookups-data`.
- `Language.Defaults` contains the shared language data.