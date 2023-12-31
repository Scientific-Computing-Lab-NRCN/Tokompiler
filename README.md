# Tokompiler: Code-Oriented Tokenizer for HPC Tasks
Tokompiler is a specialized tokenizer designed for code preprocessing and tokenization. It provides a pipeline for converting source code into a format that enhances code representation and understanding. Its primary purpose is to support the pretraining of language models for high-performance computing.

## Getting Started
To use Tokompiler, follow these steps:

Install tree-sitter:
```bash
pip install tree-sitter
```


Build the Tree-sitter parser by executing the following commands from the root directory:

```bash
cd src/tokompiler/parsers
python build_ts.py
cd -
```

Finally, run setup.py to build pip package as:
```bash
python setup.py bdist_wheel && python -m pip install dist/tokompiler-0.1-py3-none-any.whl
```

## Pipeline Overview
The pipeline of using Tokompiler consists of the following steps:

1. Convert Representation
The ConvertRepresentation class contains a function called generate_replaced that takes input code and the programming language. It returns a replaced format of the code. For example:

Original Code:
```c
int main() {
    int r[2800 + 1];
    // ... (code continues)
}
```

Replaced Format:
```c
int func_252() {
    int arr_88[num_34 + num_842];
    // ... (replaced code)
}
```

2. Lexicalization
The Lexicalization class contains a function called lexicalize that splits the code into its semantical tokens given the code and the programming language. For example:

Original Code:
```c
int func_252() {
    int arr_88[num_34 + num_842];
    // ... (code continues)
}
```

Lexicalized Tokens:
```
["int", "func", "252", "(", ")", "{", "int", "arr", "88", // ... (tokens continue)
```

3. Tokenization
The Tokenizer class defines an interface that Tokompiler implements. It includes the following functions:

tokenize: Converts code into a list of tokens.

encode: Given code, returns a list of IDs representing each token.

decode: Given a list of token IDs, returns the original code.


## Tokompiler Usage
A complete example of using Tokompiler can be found in [this example script](https://github.com/Scientific-Computing-Lab-NRCN/Tokompiler/blob/main/example.py).


## Code Perplexity
Perplexity calculation in accordance to PolyCoder (https://arxiv.org/abs/2202.13169) methods using lmppl (https://github.com/asahi417/lmppl)

you will need your model in the huggingface format. preferably pytorch only.

### Environment
The environment.yml file functions seamlessly. 

However, if you already have PyTorch installed, utilizing the partial_requirements.txt with pip is a suitable approach as well.


### LMPPL Usage

After setting up your environment and ensuring a compatible model, you can calculate perplexity by running the perplexity.py script with your chosen model and text inputs. 

Here is an example of how to use the script:

```python

# Get lexical count for the texts using 'c' language lexer (replace 'c' with the appropriate lexer for your texts)
count = get_lex_count(texts,'c')

tokenizer=AutoTokenizer.from_pretrained('gpt2')
model=AutoModelForCausalLM.from_pretrained('gpt2')
scorer = lmppl_code.LM(tokenizer=tokenizer, model_obj=model)
print(scorer.get_perplexity(texts,count))
```
