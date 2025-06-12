# SpLAT (Specimen Label Automatic Transcription)

## Overview
SpLAT is a project developed under the AI4Specimen initiative, focusing on the automated digitization of specimen labels from natural history collections. The system utilizes advanced AI and machine learning techniques to extract and standardize information from specimen labels into Darwin Core (DwC) format.

## Project Description
The project implements an automated pipeline for processing specimen images and extracting structured data from their labels. It is designed to work with the DISSCo (Distributed System of Scientific Collections) infrastructure, providing a standardized way to digitize specimen information.

## Pipeline Architecture

### Components
1. **Image Processing Pipeline**
   - Receives specimen images via webhook
   - Performs initial image processing and orientation detection
   - Handles image resizing and optimization for AI processing

2. **AI Analysis Components**
   - Label detection and text extraction
   - Language identification
   - Orientation analysis
   - Taxonomic information extraction

3. **Data Standardization**
   - Converts extracted information into Darwin Core format
   - Validates and filters data according to DwC standards
   - Outputs structured JSON data

### Supported Darwin Core Terms
The pipeline extracts the following Darwin Core terms when available:
- dwc:scientificName
- dwc:catalogNumber
- dwc:recordNumber
- dwc:recordedBy
- dwc:year
- dwc:month
- dwc:day
- dwc:dateIdentified
- dwc:identifiedBy
- dwc:verbatimIdentification
- dwc:country
- dwc:countryCode
- dwc:decimalLatitude
- dwc:decimalLongitude
- dwc:locality
- dwc:minimumElevationInMeters
- dwc:maximumElevationInMeters
- dwc:verbatimElevation

## Technical Implementation
The pipeline is implemented using n8n workflow automation platform and integrates with:
- OpenAI's GPT models for text analysis and extraction
- Image processing services for orientation and optimization
- DISSCo wrapper for standardized data handling

## Usage
The pipeline is accessible via a webhook endpoint that accepts image URIs. The system processes the images and returns standardized Darwin Core data in JSON format.

### Input Format
```json
{
  "uris": ["https://example.com/path/to/specimen/image"]
}
```

### Output Format
```json
{
  "data": {
    "dwc:scientificName": "...",
    "dwc:locality": "...",
    // ... other extracted fields
  },
  "metadata": {
    "version": "1.0"
  }
}
```

## Integration
The pipeline is designed to integrate with DISSCo infrastructure and can be used as part of larger digitization workflows. It supports:
- Batch processing of specimen images
- Standardized data output
- Error handling and validation

## Version Information
- Current Version: 1.0
- Last Updated: 2025-05-22

## Tags
- production
- DISSCo
- AI4Specimen
- SPLAT

## Development and Testing

### Workshop Timeline
1. **First Workshop (Development)**
   - Initial pipeline development
   - Implementation of n8n workflow
   - Integration with DISSCo wrapper
   - Basic functionality testing

2. **Second Workshop (Testing and Evaluation)**
   - User testing with real specimen images
   - Prompt optimization and evaluation
   - Two-stage prompt testing:
     1. Prompt1: Initial information extraction from images
     2. Prompt2: Standardization of extracted information into DwC format

### Testing Methodology
The testing phase utilized n8n Forms to collect:
- Specimen images from workshop participants
- Custom prompts for information extraction
- Custom prompts for data standardization

### Test Data
- Test images are stored in `data/AI4Specimen/` directory
- Prompt evaluation results are documented in `data/AI4Specimen - prompt evaluation.xlsx`
- Test dataset includes various specimen types and label formats

### Testing Results
The testing phase helped to:
- Optimize prompt engineering for better information extraction
- Identify common challenges in label recognition
- Improve standardization accuracy
- Gather user feedback for system improvements

### Prompt Analysis and Optimization
After the testing phase, a comprehensive analysis of all submitted prompts was conducted:

1. **Prompt Analysis**
   - Used Gemini 2.5 Pro to analyze all submitted prompts
   - Identified patterns and variations in prompt approaches
   - Condensed similar prompts into distinct variants

2. **Condensed Prompt Sets**
   - **Prompt1 (Information Extraction)**: 18 distinct variants identified
   - **Prompt2 (Standardization)**: 12 distinct variants identified
   - Results documented in `data/SPLAT prompts condensed & evaluated.xlsx`

3. **Analysis Benefits**
   - Reduced redundancy in prompt approaches
   - Identified most effective prompt patterns
   - Created a standardized set of prompts for future use
   - Improved consistency in information extraction and standardization

### Evaluation Phase
A rigorous evaluation was conducted to identify the optimal prompt combinations:

1. **Test Dataset**
   - 20 carefully selected herbarium specimen images
   - Images chosen by professional transcribers
   - Selected for their challenging nature
   - Pre-existing transcriptions available for comparison
   - Dataset documented in `data/AI4Specimen - challenging examples.csv`

2. **Evaluation Process**
   - All combinations of condensed Prompt1 and Prompt2 variants tested
   - Each image processed with each prompt combination
   - Results compared with pre-existing transcriptions
   - Term-by-term comparison using ChatGPT-4o

3. **Scoring System**
   - Better transcription: +1 point
   - Worse transcription: -1 point
   - Same quality: 0 points
   - Total score calculated for each prompt combination
   - Scores used to identify optimal prompt pairs

4. **Evaluation Benefits**
   - Objective comparison of prompt effectiveness
   - Identification of best-performing prompt combinations
   - Validation against professional transcriptions
   - Quality assurance of the automated system

### Final Optimized Prompts
After comprehensive evaluation, the following prompts were selected as the most effective:

1. **Prompt1 (Information Extraction)**
```
You are an experienced biologist specializing in describing herbarium specimens. 
You have a keen eye for detail and can describe specimens very accurately. 
Your task is to extract all information from the provided image of a herbarium specimen and create a short description containing all information from the image. 
Do not provide information on higher taxonomy beyond kingdom. Perform a literal transcription of all text visible on the specimen's label. 
The text on the label is handwritten.
```

2. **Prompt2 (Standardization)**
```
Standardize the provided information about a preserved specimen into a JSON object using exclusively valid Darwin Core terms. 
The JSON structure should follow: { "dwc:scientificName": "Value", "dwc:locality": "Value", ... }.
```

These prompts were selected based on:
- Highest overall performance in the evaluation
- Best accuracy in information extraction
- Most reliable standardization results
- Consistent performance across different specimen types

### Model Comparison
After identifying the optimal prompts, a comparison was conducted across different AI models using the same 20 challenging herbarium specimens. The evaluation used the same scoring system (+1 for better, -1 for worse, 0 for same) to compare each model's performance against the professional transcriptions.

#### Model Performance Rankings
| Model | Evaluation Score |
|-------|-----------------|
| anthropic/Claude 3.7 Sonnet | 7 |
| openai/gpt-4.5-preview | -1 |
| openai/gpt-4.1-nano | -7 |
| openai/gpt-4.1-mini | -9 |
| openai/o4-mini | -12 |
| google/gemini-2.5-pro-preview-05-06 | -17 |
| google/gemini-2.5-flash-preview-05-20 | -18 |
| anthropic/Claude 3.5 Haiku | -19 |
| openai/gpt-4o | -19 |
| xai/grok-2-vision-1212 | -29 |

#### Key Findings
- Claude 3.7 Sonnet demonstrated the best performance with a positive score of 7
- Most models performed below the baseline (professional transcription)
- Significant variation in performance across different model families
- Clear correlation between model size/capability and performance
