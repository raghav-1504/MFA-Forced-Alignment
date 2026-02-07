# MFA-Forced-Alignment
Forced alignment pipeline using Montreal Forced Aligner (MFA). Includes dataset preparation, baseline alignment, OOV handling with a custom dictionary, Praat-based analysis, and before/after TextGrid outputs for speech–text alignment.
# MFA Forced Alignment using Montreal Forced Aligner


This repository contains a complete forced alignment pipeline implemented using the **Montreal Forced Aligner (MFA)**.  
The project demonstrates baseline alignment of speech and text, identification of out-of-vocabulary (OOV) words, dictionary extension, re-alignment, and qualitative analysis of alignment outputs using **Praat**.


---


## 1. Environment Setup


### Installing Miniconda
Miniconda was used to manage dependencies and create an isolated environment for MFA.


Download Miniconda from:  
https://docs.conda.io/en/latest/miniconda.html


### Creating and activating the MFA environment


conda create -n mfa python=3.10
conda activate mfa
conda install -c conda-forge montreal-forced-aligner

Verify installation:

mfa version

All commands below were executed inside the activated (mfa) environment.

2. Dataset Preparation

The dataset consists of speech audio files (.wav) and corresponding transcripts (.txt).

Dataset structure
data/
├── F2BJRLP1.wav
├── F2BJRLP1.txt
├── F2BJRLP2.wav
├── F2BJRLP2.txt
└── ...
Preparation rules

Each audio file has a transcript file with the same base filename

Audio files are mono, 16 kHz WAV

Transcripts were normalized before alignment:

Converted to uppercase

Punctuation removed

Abbreviations expanded where necessary

Tokens adjusted to reflect spoken content

Transcript normalization ensures compatibility with MFA’s pronunciation dictionary.

3. Downloading Pretrained Models

The following pretrained models were used:

Pronunciation dictionary: english_us_arpa

Acoustic model: english_us_arpa

Download them using:

mfa model download dictionary english_us_arpa
mfa model download acoustic english_us_arpa
4. Baseline Forced Alignment (Before OOV Handling)

Baseline forced alignment was performed using the pretrained dictionary and acoustic model.

mfa align data english_us_arpa english_us_arpa aligned
Output

Word-level and phoneme-level TextGrid files were generated in the aligned/ directory.

During inspection in Praat, several proper nouns and broadcast-specific terms were aligned as spn, indicating out-of-vocabulary or acoustically unclear regions.

5. OOV Identification and Handling

Out-of-vocabulary (OOV) words were identified by inspecting the baseline alignment outputs in Praat.

Approach

Proper nouns and abbreviations not covered by the baseline dictionary were identified

A custom pronunciation dictionary was created by adding ARPAbet transcriptions for the identified OOV words

G2P-based pronunciation generation was explored, but manual dictionary extension was chosen due to the small number of OOV words and the need for controlled pronunciations

6. Forced Alignment After OOV Handling

Forced alignment was re-run using the extended dictionary.

mfa align data custom.dict english_us_arpa aligned_after_oov
Output

Updated TextGrid files were generated in the aligned_after_oov/ directory

Alignment results were inspected again using Praat

7. Alignment Analysis and Observations

Common vocabulary items showed reasonable alignment at word and phoneme levels

Some proper nouns continued to appear as spn even after dictionary extension

This behavior is attributed to:

Rapid broadcast-style speech

Coarticulation effects

Limited acoustic evidence for clear segmentation

These results highlight that dictionary coverage alone may be insufficient when acoustic cues are weak.

8. Visualization using Praat

Alignment outputs were inspected using Praat.

Screenshots illustrating alignment behavior before and after OOV handling are provided in:

report_images/
├── before_oov.png
├── after_oov.png

These images were used for qualitative comparison and analysis.

9. Outputs

aligned/ – TextGrid files from baseline alignment
aligned_after_oov/ – TextGrid files after OOV handling

MFA_Forced_Alignment_Report.docx – Detailed report summarizing methodology and observations

10. Conclusion

This project demonstrates a complete forced-alignment workflow using Montreal Forced Aligner. While MFA performs well for common vocabulary, proper nouns in fast broadcast speech remain challenging even after dictionary extension. The assignment provides practical insight into forced alignment, OOV handling, and the limitations imposed by acoustic characteristics.
---
