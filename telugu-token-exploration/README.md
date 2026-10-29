A small NLP project that compares how pretrained tokenizers split Telugu text, and what that reveals about Telugu's morphology and script.

Telugu is agglutinative: one word can carry a stem plus several suffixes (tense, person, case). It also uses an abugida script, where vowel signs and conjunct consonants are separate Unicode code points. Tokenizers that never saw much Telugu break words into many tiny pieces, which means higher cost, a shorter effective context window, and often lower model quality.

## Implementations:
- Compared five pretrained tokenizers:
  - mBERT (`bert-base-multilingual-cased`), WordPiece
  - XLM-R (`xlm-roberta-base`), SentencePiece
  - MuRIL (`google/muril-base-cased`), WordPiece
  - IndicBERT (`ai4bharat/IndicBERTv2-MLM-only`), SentencePiece
  - GPT-2 (`gpt2`), byte-level BPE, as an English-centric baseline
- Ran them on 5 Telugu sentences with different grammar
- Measured **fertility** (tokens per word; lower is better)
- Inspected splits of suffix-heavy words like `నేర్చుకుంటున్నాను` ("I am learning")

**Key findings** :
- Lowest fertility: indicBERT at 1.39
- Highest fertility: GPT2 at 23.64
- Example: `నేర్చుకుంటున్నాను` is 17 characters (51 UTF-8 bytes) and became 83 tokens in GPT-2 versus 5 in indicBERT

## Limitations
- Only 5 sentences, so results are illustrative, not statistically rigorous
- Fertility measures efficiency, not downstream model quality
- Whitespace-based word counting is a rough proxy for Telugu words
