# PhishGuard

## Overview
Phishing detection system using GPT-4o and ModernBERT models to analyze emails and URLs.

## Files and Purpose

### Demo_CodeIntegration.ipynb (Main System)
Complete working PhishGuard implementation containing:
- **URLExtractor**: Finds and analyzes all URLs in emails (checks for IP addresses, URL shorteners, suspicious domains)
- **EmailAnalyzer**: Connects to GPT-4o API to classify email content as phishing or safe
- **URLClassifier**: Loads ModernBERT model to classify each URL as malicious or benign
- **DecisionFusion**: Combines results using OR logic (if email OR any URL is phishing = PHISHING)
- **PhishGuardSystem**: Main orchestrator that runs all components
- **Test Suite**: 7 scenarios testing all possible combinations

### ModernBERT.ipynb (URL Model Training)
- Trains ModernBERT model on phishing URL dataset
- Saves trained model to Google Drive
- Used by Demo_CodeIntegration.ipynb for URL classification

### Gpt4o.ipynb (Email Model Training)  
- Fine-tunes GPT-4o on phishing email dataset
- Creates model: `ft:gpt-4o-mini-2024-07-18:personal::C1GDgRus`
- Used by Demo_CodeIntegration.ipynb for email analysis

## How Demo_CodeIntegration.ipynb Works

1. **Initialize System**
   - Loads ModernBERT from Google Drive
   - Connects to OpenAI API with your key
   - Sets up all analyzers

2. **Process Email**
   - EmailAnalyzer sends email to GPT-4o → gets phishing score
   - URLExtractor finds all URLs in the email
   - URLClassifier checks each URL with ModernBERT → gets URL scores

3. **Decision Logic**
   - Uses OR logic: Email=PHISHING OR URL=PHISHING → PHISHING
   - Calculates combined score: (Email × 0.6) + (URL × 0.4)
   - Assigns risk level: CRITICAL, HIGH, PHISHING, or SAFE

4. **Test Scenarios**
   - Runs 7 tests automatically:
     - Phishing email (no URLs)
     - Safe email (no URLs)  
     - Phishing email + safe URLs
     - Phishing email + phishing URLs
     - Safe email + phishing URLs
     - Safe email + safe URLs
     - Safe email + mixed URLs

## Requirements

- Google Colab with GPU
- OpenAI API key
- ModernBERT model: https://drive.google.com/drive/folders/1o8k7QM83ToYyUQfCsXrZ7D8wzcLUDsPP?usp=sharing

## How to Run

1. Upload Demo_CodeIntegration.ipynb to Google Colab
2. Set GPU runtime
3. Mount Google Drive with ModernBERT model
4. Add OpenAI API key
5. Run all cells - system will automatically test all scenarios

## Configuration in Demo

- GPT Model: `ft:gpt-4o-mini-2024-07-18:personal::C1GDgRus`
- ModernBERT Path: `/content/drive/MyDrive/dhanya_modernBERT_unzipped`
- Threshold: 0.5 (scores above this = phishing)
- Weights: 60% email, 40% URLs