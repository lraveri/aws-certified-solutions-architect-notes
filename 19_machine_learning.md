# Machine Learning

## Amazon Rekognition

Amazon Rekognition is a machine learning service that allows you to analyze images and videos to detect various elements such as:

- **Objects**, **people**, **text**, and **scenes**
- **Facial analysis** and **facial search** for:
  - User verification
  - People counting

You can also:
- Create a database of "familiar faces"
- Compare faces against a celebrity database

### Use Cases

- **Labeling**: Automatically assign labels to images and videos.
- **Content Moderation**: Detect inappropriate content.
- **Text Detection**: Extract text from images and video frames.
- **Face Detection and Analysis**: Determine attributes like gender, age range, and emotions.
- **Face Search and Verification**: Compare a face against a collection for authentication or identification.
- **Celebrity Recognition**: Identify well-known individuals in media.
- **Pathing**: Analyze movement paths, useful for scenarios like sports game analysis.

## Amazon Rekognition – Content Moderation

Amazon Rekognition can analyze images and videos to detect **inappropriate**, **unwanted**, or **offensive content**.

### Common Use Cases
- **Social media**, **broadcast media**, **advertising**, and **e-commerce** platforms
- Ensures a **safer user experience**

### Key Features
- **Minimum Confidence Threshold**: Define the confidence level required for a piece of content to be flagged.
- **Manual Review with Amazon A2I (Augmented AI)**: Content flagged by Rekognition can be reviewed by humans for further validation.
- **Regulatory Compliance**: Helps businesses adhere to content guidelines and legal requirements.

The moderation process can be summarized as:
1. Rekognition analyzes media and returns labels with associated **confidence levels**.
2. If confidence exceeds the defined **threshold**, the content is **flagged**.
3. Optionally, flagged content can be **sent for manual review** using **Amazon A2I**.

## Amazon Transcribe

Amazon Transcribe is a service that automatically converts **speech to text** using deep learning.

### Key Features

- Uses **Automatic Speech Recognition (ASR)** for fast and accurate transcription
- Can **automatically redact Personally Identifiable Information (PII)**
- Supports **Automatic Language Identification** for audio with multiple languages

### Use Cases

- Transcribe **customer service calls**
- Automate **closed captioning** and **subtitling**
- Generate **metadata** for media assets to build a fully searchable archive

## Amazon Polly

Amazon Polly is a service that turns **text into lifelike speech** using deep learning technologies.

### Key Features

- Converts text to speech in a natural-sounding voice
- Enables the development of **applications that talk**

Typical applications include voice-enabled apps, accessibility tools, and automated customer support systems.

## Amazon Polly – Lexicon & SSML

Amazon Polly provides advanced customization for speech output using **pronunciation lexicons** and **SSML (Speech Synthesis Markup Language)**.

### Pronunciation Lexicons

- Customize how specific words are pronounced.
- Examples:
  - Stylized words: `St3ph4ne` → “Stephane”
  - Acronyms: `AWS` → “Amazon Web Services”
- Lexicons can be **uploaded** and referenced in the `SynthesizeSpeech` operation.

### SSML (Speech Synthesis Markup Language)

SSML allows enhanced control over speech output:
- **Emphasize** specific words or phrases
- Use **phonetic pronunciation**
- Add **breathing sounds** or **whispering**
- Apply the **Newscaster speaking style** for a more natural and professional tone

## Amazon Translate

Amazon Translate provides **natural and accurate language translation** using machine learning.

### Key Features

- Localize content such as **websites** and **applications** for international users
- Efficiently translate **large volumes of text**

## Amazon Lex & Connect

### Amazon Lex

Amazon Lex is the same technology that powers **Alexa**, and it enables you to build conversational interfaces using:

- **Automatic Speech Recognition (ASR)**: Converts speech to text
- **Natural Language Understanding (NLU)**: Recognizes user intent from input text or speech

Use cases:
- Build **chatbots** and **call center bots** for customer service automation

### Amazon Connect

Amazon Connect is a **cloud-based virtual contact center** that allows:

- Receiving and routing phone calls
- Creating **contact flows** for caller interactions
- Integration with **CRM systems** or other **AWS services**
- No upfront payments, and costs up to **80% less** than traditional contact centers

### Example Flow

1. A user makes a **phone call**
2. **Amazon Connect** receives the call and invokes **Amazon Lex**
3. Lex recognizes the **intent** (e.g., "schedule an appointment")
4. A **Lambda function** is triggered to update the **CRM**

## Amazon Comprehend

Amazon Comprehend is a **fully managed, serverless NLP (Natural Language Processing)** service that uses machine learning to extract insights and relationships from text.

### Key Features

- Detects the **language** of the input text
- Extracts **key phrases**, **places**, **people**, **brands**, and **events**
- Performs **sentiment analysis** (positive, negative, neutral, or mixed)
- Analyzes text using **tokenization** and **part-of-speech tagging**
- Automatically groups a collection of text files by **topic modeling**

### Use Cases

- Analyze **customer interactions** (e.g., emails) to understand sentiment and causes of satisfaction or dissatisfaction
- Automatically **categorize and group articles** based on underlying topics discovered by Comprehend

## Amazon Comprehend Medical

Amazon Comprehend Medical is an NLP service that extracts **useful medical information** from unstructured clinical text.

### Supported Text Types

- **Physician’s notes**
- **Discharge summaries**
- **Test results**
- **Case notes**

### Key Features

- Uses NLP to detect **Protected Health Information (PHI)** via the `DetectPHI` API
- Can be integrated with:
  - **Amazon S3** to store and analyze documents
  - **Amazon Kinesis Data Firehose** to process real-time data
  - **Amazon Transcribe** to convert spoken patient narratives into text, which can then be analyzed

This service helps healthcare providers process and analyze clinical documentation more efficiently and securely.

## Amazon SageMaker

Amazon SageMaker is a **fully managed service** that enables developers and data scientists to build, train, and deploy machine learning (ML) models at scale.

### Key Benefits

- Eliminates the complexity of provisioning and managing infrastructure for ML
- Centralizes all steps of the ML lifecycle: **data preparation**, **training**, **tuning**, and **deployment**

### Example ML Workflow (Predicting Exam Score)

1. **Historical Data**:
   - Number of years of experience in IT
   - Number of years of experience with AWS
   - Time spent on the course
   - Corresponding exam scores (labels)

2. **Model Training and Tuning**:
   - Build and train an ML model using the historical data

3. **Model Application**:
   - Feed new input data to the trained model
   - Get a prediction, e.g., “PASS WITH 906”

SageMaker helps automate and scale this entire pipeline.

## Amazon Kendra

Amazon Kendra is a **fully managed intelligent search service** that uses machine learning to enable natural language search across a variety of documents.

### Key Features

- Extracts answers from documents in various formats:
  - Text
  - PDF
  - HTML
  - PowerPoint
  - MS Word
  - FAQs
- Supports **natural language queries**, e.g., “Where is the IT support desk?”
- Performs **incremental learning** from user interactions to improve result relevance
- Allows **manual tuning** of search results based on:
  - Data importance
  - Freshness
  - Custom rules

### Supported Data Sources

- **Amazon S3**
- **Amazon RDS**
- **Google Drive**
- **Microsoft SharePoint**
- **Microsoft OneDrive**
- **3rd party applications**
- **Custom sources via API**

Amazon Kendra builds a **machine-learning-powered knowledge index** to deliver relevant answers directly from your content.

## Amazon Personalize

Amazon Personalize is a **fully managed machine learning service** that enables you to deliver **real-time personalized recommendations**.

### Key Features

- Provides recommendations such as:
  - Personalized product suggestions and re-ranking
  - Customized direct marketing content
- Uses the **same technology** as Amazon.com
- Easily integrates with:
  - **Websites and mobile apps**
  - **SMS and email marketing systems**
- Allows implementation in **days**, without needing to build, train, or deploy ML models manually

### Example

If a user buys gardening tools, Amazon Personalize can recommend related products to buy next.

### Architecture Overview

- **Amazon S3** is used to provide historical data
- Real-time data can be integrated via the **Amazon Personalize API**
- Results can be consumed by web apps, mobile apps, email, or SMS systems

### Use Cases

- **Retail**: Improve product discovery
- **Media & Entertainment**: Suggest content based on user preferences

## Amazon Textract

Amazon Textract is an AI and ML-powered service that automatically **extracts text, handwriting, and structured data** from scanned documents.

### Key Features

- Extracts data from **forms** and **tables**
- Reads and processes various document types including:
  - **PDFs**
  - **Images**
- Can handle both **printed** and **handwritten** text

### Use Cases

- **Financial Services**: Invoices, financial statements, and reports
- **Healthcare**: Medical records, insurance claims
- **Public Sector**: Tax forms, ID documents, passports

Amazon Textract outputs structured data, enabling further analysis or integration into backend systems.

## AWS Machine Learning – Summary

### Core Services and Their Capabilities

- **Amazon Rekognition**: Face detection, image labeling, celebrity recognition
- **Amazon Transcribe**: Converts audio to text (e.g., subtitles)
- **Amazon Polly**: Converts text to lifelike speech
- **Amazon Translate**: Natural and accurate language translation
- **Amazon Lex**: Build conversational chatbots with speech and text
- **Amazon Connect**: Cloud-based virtual contact center
- **Amazon Comprehend**: Natural Language Processing for text analysis
- **Amazon SageMaker**: Full ML lifecycle for developers and data scientists
- **Amazon Kendra**: Intelligent document search with ML
- **Amazon Personalize**: Real-time personalized recommendations
- **Amazon Textract**: Automatically extracts text and data from documents
