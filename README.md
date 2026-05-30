# LLM Token Explorer

A small CLI assignment to make today's lesson concrete: **everything is tokens, and tokens cost money**.

You'll build a tool that takes some text and shows you what the OpenAI tokenizer actually does to it — how it splits the text, how many tokens it produces, and what that would cost at GPT-4o pricing.

## Setup

```bash
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
```

The only dependency is [`tiktoken`](https://github.com/openai/tiktoken), OpenAI's open-source tokenizer.

## Requirements

### 1. Tokenize a single string

```bash
python token_explorer.py "The developer wrote Python code."
```

Output should include:
- The total token count
- Each token with its integer ID, one per line
- The estimated **input** cost at GPT-4o pricing (currently ~$2.50 per 1M input tokens)

Use the `cl100k_base` encoding (`tiktoken.get_encoding("cl100k_base")`), which is what GPT-4o uses.

### 2. Read from a file

```bash
python token_explorer.py -f some_text.txt
```

Same output shape, but for the file's contents.  Pick any text file lying around your system — your notes, a chunk of a README, a snippet of code.

### 3. Round-trip sanity check

After encoding, decode each token ID back into a string with `encoder.decode([id])` and print it alongside the ID.  Confirms that the IDs really do map back to text.

## Things to think about
- Why does `"the"` get one token but `"tokenization"` gets split?  Open the [OpenAI Tokenizer Playground](https://platform.openai.com/tokenizer) and check a few words — what kinds of words tend to get split, and what kinds stay whole?
- At GPT-4o's ~$2.50 per 1M **input** tokens, how much would it cost to send a 5,000-word document?  How about the entire text of *Moby Dick* (~210,000 words)?
- Output tokens are more expensive than input tokens (~$10 vs ~$2.50 per 1M for GPT-4o).  Why might that be?
- If you ran the same string through Claude's tokenizer instead of GPT-4o's, would you expect the token count to be exactly the same?  Why or why not?

## Stretch
- Add a `--cost-out N` flag that also calculates the estimated **output** cost assuming the model returns `N` tokens.
- Add a `--compare path1 path2` mode that tokenizes two files and prints token-count + cost side by side.
- Build a tiny "prompt diet" mode: read a file, find the **5 longest** tokens (by character length), and suggest the user check whether they really need them.
- Plot token frequency: which tokens appear most often across a folder of text files?
- Swap the encoding to `o200k_base` (the newer GPT-4o encoding) and compare counts.

> Stuck? Have a code error? Use the ["4 Before Me"](https://docs.google.com/document/d/1nseOs5oabYBKNHfwJZNAR7GlU0zkZxNagsw63AD7XV0/edit) debugging checklist to help you solve it!
