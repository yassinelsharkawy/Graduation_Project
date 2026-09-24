# PolySumm: A Multimodal Retrieval-Augmented Framework for Scientific Paper Understanding and Summarization

## Project Overview

### Problem Statement
The exponential growth of scientific literature presents a significant challenge for researchers seeking to efficiently extract key information. Traditional summarization approaches primarily focus on text, often neglecting critical information embedded within figures and tables. This limitation leads to incomplete understanding and inefficient knowledge acquisition, particularly in fields where visual data representations are crucial.

### Objectives
This project aimed to develop a comprehensive multimodal summarization system for scientific papers. The system integrates information from text, figures, and tables to generate more complete and informative summaries. Our specific objectives included:

1.  **Designing a Robust Pipeline:** To extract structured content from scientific PDF documents.
2.  **Implementing Specialized Summarization Models:** For different content types (text, figures, and tables).
3.  **Integrating Components within a RAG Framework:** To leverage Retrieval-Augmented Generation for enhanced summarization.
4.  **Providing an Accessible User Interface:** To enable researchers to easily utilize this technology.

## Demo Video

A short demonstration of the Multimodal Scientific Paper Summarization Graduation Project system in action is available below:

PolySumm is a research-oriented system for helping readers navigate scientific papers by combining information from text, figures, and tables. The project explores how document structure, specialized multimodal models, and retrieval-augmented generation can work together to produce useful paper summaries and question-answering experiences.

## Research Motivation

Scientific papers communicate evidence through multiple modalities. Text describes the research question and methodology, figures communicate visual results, and tables contain quantitative comparisons. A text-only summarizer can miss important evidence when these modalities are treated independently or ignored.

PolySumm addresses this problem with a modular pipeline that extracts scientific documents, processes each modality with a suitable model, and exposes the resulting information through summarization and question-answering services.

## What This Repository Implements

- PDF processing and structured extraction of text, figures, and tables
- Modality-specific summarization components for scientific documents
- PEGASUS-based text summarization service
- Vision-language figure captioning service using a PaliGemma model and a local PEFT adapter
- Table summarization service using Qwen2-VL-2B-Instruct
- A custom Retrieval-Augmented Generation (RAG) pipeline
- An Azure OpenAI-based RAG pipeline for comparison and experimentation
- ChromaDB-backed vector retrieval and document metadata handling
- FastAPI services for ingestion, summarization, and question answering
- A React frontend for uploading papers, monitoring processing, reviewing results, and chatting with documents

## System Architecture

```text
Scientific Paper in a PDF Format
         |
         v
Paper Extraction and Layout Analysis
         |
         +------------------+------------------+
         |                  |                  |
     Text              Figures             Tables
         |                  |                  |
 PEGASUS       PaliGemma + PEFT       Qwen2-VL
         |                  |                  |
         +------------------+------------------+
                                                |
                                                v
                    Structured Multimodal Representation
                                                |
                                                v
             Chunking, Embeddings, and Vector Retrieval
                                                |
                         +----------+----------+
                         |                     |
             Custom RAG             Azure OpenAI RAG
                         |                     |
                         +----------+----------+
                                                |
                                                v
                Summaries, Answers, and Source Context
                                                |
                                                v
                                 React Frontend
```

## Research and Engineering Contributions

### 1. Modality-aware document processing

The repository separates text, figures, and tables during PDF processing. This creates a structured representation that can preserve information that would otherwise be lost in a plain-text conversion.

### 2. Specialized model selection

Each modality is handled by a model or service suited to its representation:

- **Text:** PEGASUS-based abstractive summarization
- **Figures:** PaliGemma-based image captioning with a local PEFT adapter
- **Tables:** Qwen2-VL image-to-text summarization

This design makes the pipeline modular and allows individual components to be evaluated or replaced independently.

### 3. Retrieval-grounded interaction

The RAG services ingest scientific papers, create retrievable chunks and embeddings, and use relevant document context when answering questions or generating summaries. The OpenAI RAG implementation also exposes methodology-oriented question answering and source-aware document interaction through a FastAPI API.

### 4. Comparative system design

The repository contains both a custom RAG implementation and an Azure OpenAI-based implementation. Keeping these paths separate provides a practical basis for studying trade-offs among model ownership, deployment requirements, retrieval design, and generation quality.

### 5. Research prototype to usable interface

The React frontend connects the research components to an application workflow. It includes routes for paper upload, processing, results, document chat, and project information, making the system suitable for demonstrations and continued experimentation.

## Repository Structure

| Directory | Purpose |
| --- | --- |
| `Paper-Extractor/` | PDF upload API and document extraction pipeline |
| `Summarization_Model/` | PEGASUS-based scientific text summarization API |
| `Figure_Summarization/` | Figure captioning service using PaliGemma and PEFT |
| `Table_Summarization/` | Table summarization service using Qwen2-VL |
| `Working_RAG/` | Custom RAG implementation and supporting modules |
| `OpenAI_RAG/` | FastAPI RAG system using Azure OpenAI and ChromaDB |
| `Paper_Classification/` | Scientific paper classification experiments |
| `Frontend/` | React client for the PolySumm workflow |
| `Documentation/` | Project documentation and supporting material |
| `PolySumm-Demo.mp4` | Project demonstration video |

## Technology Stack

- **Frontend:** React, React Router, Bootstrap, Axios, Framer Motion
- **Backend APIs:** Python, FastAPI, Uvicorn, Pydantic
- **Document processing:** PyMuPDF, PyPDF2, PDF-to-image utilities, PaddleOCR-related tooling
- **Language models:** PEGASUS, transformer-based models, Azure OpenAI
- **Vision-language models:** PaliGemma and Qwen2-VL
- **Parameter-efficient adaptation:** PEFT adapters for the figure captioning component
- **Retrieval:** LangChain, ChromaDB, embeddings, semantic chunking
- **Data and image processing:** NumPy, pandas, Pillow, PyTorch

## Demonstration

A demonstration video is included in the repository:

[//]: # (Placeholder for demo video)
https://github.com/yassinelsharkawy/Graduation_Project/blob/4ebe0f7f1578c031b4f25a01483a781d6ede98ca/PolySumm-Demo.mp4

## Running the Components

The repository is organized as several independently runnable services rather than one single package. Each major component contains its own Docker or dependency configuration where applicable.

### OpenAI RAG service

```bash
cd OpenAI_RAG
pip install -r requirements.txt
python start.py
```

The service provides FastAPI documentation at `http://localhost:8000/docs` when it is running. Azure OpenAI configuration is supplied through environment variables; consult `OpenAI_RAG/README.md` for the available settings and endpoints.

### React frontend

```bash
cd Frontend
npm install
npm start
```

The frontend is based on Create React App and runs at `http://localhost:3000` by default.

### Other services

The extraction, text summarization, figure summarization, table summarization, and custom RAG components have separate entry points and dependency files. Their local setup details are documented in the corresponding directory.

## Current Scope and Reproducibility Notes

This repository represents a graduation-project research prototype. Model loading may require substantial memory, a compatible GPU, downloaded model weights, and credentials for external services. Results can depend on model versions, hardware, prompts, and runtime configuration.

The repository documents implemented components and intended experiments; it does not claim a universal benchmark result. Quantitative comparisons should be reproduced with a defined dataset, evaluation protocol, and fixed hardware and model settings before drawing general conclusions.

## Future Research Directions

- Evaluate each modality and the complete pipeline with reproducible datasets and metrics.
- Study multimodal fusion strategies for combining textual, visual, and tabular evidence.
- Improve handling of mathematical notation, complex layouts, and OCR uncertainty.
- Support multi-paper synthesis for literature reviews and survey preparation.
- Investigate cross-lingual scientific document processing.
- Add user feedback and controllable summary depth while preserving source grounding.
- Improve deployment efficiency and reduce memory requirements for local inference.

## Project Context

PolySumm was developed as a graduation project focused on multimodal document understanding, scientific text summarization, and retrieval-augmented generation. The repository is intended to support technical review, reproducible experimentation, and future research development.
