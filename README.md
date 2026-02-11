# Scanned PDF to Epub Converter

[English](README.md) | [中文](README_zh.md)

This tool converts scanned PDF books into clean, readable EPUB ebooks using the Baidu PaddleOCR Layout Analysis API.

## Features

- **High-Quality Layout Analysis**: Uses PaddleOCR to intelligently detect paragraphs, headers, images, and tables.
- **Smart Chapter Splitting**: Automatically detects chapter headings from OCR output. An interactive TOC review lets you confirm, remove, or adjust detected chapters before generating the EPUB.
- **Cover Image**: Automatically extracts the first page of the PDF as the EPUB cover.
- **Metadata Support**: Interactively prompts for book title and author based on OCR'd first-page text, or accepts them via CLI arguments.
- **Image Embedding**: Preserves images from the original PDF.
- **Clean Output**: Removes headers, footers, and page numbers for a seamless reading experience.
- **Robustness**:
  - **Checkpointing**: Saves progress after every chunk. If interrupted, simply re-run to resume.
  - **Rate Limiting**: Includes delays to respect API limits.
  - **Retry Logic**: Automatically retries failed API requests.

## Prerequisites

- Python 3.8+
- PaddleOCR API Token

## Getting an API Token

1. Log in to [Baidu AIStudio (飞桨星河社区)](https://aistudio.baidu.com/).
2. Go to the **"Applications"** or **"Online Models"** section (Layout Parsing).
3. Find the **"PaddleOCR"** or **"Document Analysis"** API.
4. Copy your private **API Token** from your user profile or application settings dashboard.
    *Note: Ensure you have sufficient quota (pages/day) for your usage.*

## Installation & Usage (Recommended: `uv`)

This project uses [uv](https://github.com/astral-sh/uv) for fast, reliable dependency management.

1. **Clone the repository**:

    ```bash
    git clone https://github.com/yourusername/pdf2epub-paddle.git
    cd pdf2epub-paddle
    ```

2. **Install `uv`** (if you haven't already):

    ```bash
    # On macOS/Linux
    curl -LsSf https://astral.sh/uv/install.sh | sh
    ```

3. **Set up your API token**:

    ```bash
    cp .env.example .env
    ```

    Edit `.env` and add your token:

    ```
    PADDLE_API_TOKEN=your_api_token_here
    ```

4. **Run directly with `uv`** (handles virtualenv & dependencies automatically):

    ```bash
    uv run pdf2epub_paddle.py /path/to/your/book.pdf
    ```

    The tool will display the OCR'd text from the first page and prompt you to enter the book title and author.

    You can also provide metadata directly via CLI arguments:

    ```bash
    uv run pdf2epub_paddle.py --title "Book Title" --author "Author Name" /path/to/your/book.pdf
    ```

    To specify a custom output path:

    ```bash
    uv run pdf2epub_paddle.py --output /path/to/output.epub /path/to/your/book.pdf
    ```

    To skip the interactive TOC review and use automatic chapter detection:

    ```bash
    uv run pdf2epub_paddle.py --auto-toc /path/to/your/book.pdf
    ```

    To produce a single-chapter EPUB with no chapter splitting:

    ```bash
    uv run pdf2epub_paddle.py --no-toc /path/to/your/book.pdf
    ```

### Alternative: Standard Pip

If you prefer standard pip:

1. Create a virtual environment:

    ```bash
    python -m venv .venv
    source .venv/bin/activate
    ```

2. Install dependencies:

    ```bash
    pip install .  # Installs from pyproject.toml
    ```

3. Set up your API token:

    ```bash
    cp .env.example .env
    # Edit .env and add your token
    ```

4. Run:

    ```bash
    python pdf2epub_paddle.py /path/to/your/book.pdf
    ```

> **Note**: You can also set the token via environment variable directly: `export PADDLE_API_TOKEN='your_token'`. The `.env` file is loaded automatically but will not override an existing environment variable.

## Configuration

- **Chunk Size**: Default is 5 pages per chunk to ensure stability. You can modify `CHUNK_SIZE` in the script if you have a stable connection and higher limits.
- **Timeout**: Default timeout is 180s per request.

## License

[MIT License](LICENSE)
