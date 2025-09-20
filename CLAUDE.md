# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview
This repository contains the Standardized Project Gutenberg Corpus (SPGC) tools - Python scripts for downloading, processing, and analyzing texts from Project Gutenberg. The codebase processes raw UTF-8 books through a pipeline that removes headers, tokenizes text, and generates word counts.

## Commands

### Environment Setup
```bash
# Activate virtual environment (if exists)
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### Data Operations
```bash
# Download/update Project Gutenberg data
python get_data.py

# Process downloaded books (cleanup, tokenize, count)
python process_data.py

# Download subset of books matching pattern
python get_data.py -p "PG100*"

# Process subset of books
python process_data.py -p "PG100*"
```

## Architecture

### Data Pipeline
The corpus processing follows a 4-stage pipeline:
1. **Mirror** (`data/.mirror/`) - Raw downloads from Project Gutenberg via rsync
2. **Raw** (`data/raw/`) - UTF-8 encoded books as downloaded
3. **Text** (`data/text/`) - Books with headers/legal notices removed
4. **Tokens** (`data/tokens/`) - Tokenized text, one token per line
5. **Counts** (`data/counts/`) - Word frequency counts

### Core Modules
- `get_data.py` - Downloads books and metadata from Project Gutenberg mirror
- `process_data.py` - Processes raw texts through the pipeline
- `src/pipeline.py` - Core processing logic for individual books
- `src/cleanup.py` - Header/footer removal using pattern matching
- `src/tokenizer.py` - NLTK-based tokenization with language support
- `src/metadataparser.py` - Parses RDF metadata files to create metadata DataFrame
- `src/bookshelves.py` - Handles Project Gutenberg bookshelf categorization

### Key Processing Details
- Books are identified by PG numbers (e.g., PG12345)
- Processing skips already-processed files unless overwrite flag is set
- Language-specific tokenization using NLTK Punkt tokenizer
- Metadata stored as pickled pandas DataFrames in `metadata/`
- Supports parallel processing via pattern matching

## Dependencies
- Python 3.x (2.x not supported)
- pandas, numpy for data handling
- lxml for XML parsing
- nltk for text tokenization
- rsync for data synchronization