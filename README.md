# Music EEG

This project explores the neural channel capacity of the human brain in processing music and speech using EEG data and a task-optimized, interpretable CNN.

Inspired by previous studies in auditory cognition and information theory, we investigate how the brain allocates resources when decoding musical genres versus linguistic content under varying playback conditions. The framework integrates EEG-based experiments, linguistic feature analysis, and a dual-pathway CNN model to simulate auditory perception and establish parallels between biological and computational processing limits.

---

## Project Structure

```bash
music_eeg/
├── data/                # Dataset and labels (syllable)
│   ├── eeg/             # EEG data
│   ├── label/           # JSON label mappings (syllable, genre)
│   └── music_audio/     # Test audio stimuli (.mp3) and corresponding TextGrid annotations
│
├── modules/             # Core processing modules
│   └── Demo/            # Jupyter notebooks for demo: TTS for dataset generation, prediction, feature extraction
│
├── docs/                # Documentation and architecture diagrams
|
└── README.md            # Project overview and instructions
```

---

## Overview

- AI-synthesized 2-minute Mandarin song: "智联家园"
- Playback at 4 speeds: 1×, 2×, 3×, 4×
- High-density 256-channel EEG from 36 native Mandarin speakers
- 263 syllables per speed, segmented using Whisper Large-V3
- Syllable-level CNN features aligned to EEG via multivariate temporal response function (mTRF)

---

## Key Features

| Module | Description |
|--------|-------------|
| **Stimulus Processing** | Whisper V3 used to segment 263 syllables at each speed |
| **Audio Features** | Cochleagram features extracted and fed to fine-tuned CNN |
| **EEG Recording** | 256-channel EEG while listening at 4 playback speeds |
| **CNN Layers** | Activations from `conv1` to `conv5_M` and `conv5_S` |
| **mTRF Analysis** | Align EEG with CNN-derived features (PCA-reduced) |
| **Statistical Tests** | Cluster-based permutation (10k reps) using MNE & Eelbrain |
| **Performance Evaluation** | Top-5 accuracy on syllable classification across speeds |

---

## Experimental Documentation

### Experimental Setup

#### Hardware and Software Environment
All experiments were conducted on a Linux-based server equipped with:

- **Operating System**: Ubuntu 20.04 LTS  
- **CPU**: Intel Xeon Gold 6348  
- **GPU**: NVIDIA A100 40GB (CUDA 11.7)  
- **Memory**: 256 GB DDR4  
- **Framework**: TensorFlow 2.11.0 with Keras API  
- **Audio Processing Libraries**: pycochleagram, librosa, praat-parselmouth  

#### Experimental Groups
fine-tuning setups:

- **Syllable Recognition**: 84-class syllable classification  

#### Input and Output Format
- **Input**: 256×256 cochleagram images from 16 kHz Mandarin audio clips  
- **Preprocessing**: filterbank analysis, envelope compression, Lanczos interpolation  
- **Output**: Softmax probability over 63 or 84 categories (task-dependent)  

#### Test Conditions
- **Clean**: Original speech (TTS + human)    9 × 84
- **Noisy**: Augmented with THCHS-30 additive noise with SNR (1,5,10,15 dB), pitch shifting(±1,2,3), and time-stretching(0.8,0.9,1.1,1.2,1.3,1.4,1.5) 

### Data Collection and Annotation Procedures

#### Audio Sources
Two types of speech data were used for training and evaluation:

- **Synthetic Speech**:  
  Generated using Edge TTS (Microsoft Azure) engines. Both Mandarin Chinese female and male voices were used for diversity.
  Edge TTS Chinese Voices Used:

| Voice Name        | Gender | Locale    | Description                            |
|-------------------|--------|-----------|----------------------------------------|
| XiaoxiaoNeural    | Female | zh-CN     | Warm, natural – suitable for narration or news |
| XiaoyiNeural      | Female | zh-CN     | Lively – suitable for stories or cartoons |
| YunxiNeural       | Male   | zh-CN     | Professional – suitable for broadcasts |
| YunjianNeural     | Male   | zh-CN     | Energetic – suitable for sports or dialogue |
| YunyangNeural     | Male   | zh-CN     | Clear and steady – suitable for general content |
| HsiaoChenNeural   | Female | zh-TW     | Friendly and expressive – Taiwanese Mandarin |
| HsiaoYuNeural     | Female | zh-TW     | Pleasant and casual – Taiwanese Mandarin |


- **Human Recordings**:  
  A native Chinese volunteer read and recorded selected song lyrics in a quiet environment. Recordings were performed using high-quality USB microphones at a 16 kHz sampling rate.

#### Text Materials
The linguistic content used for all speech stimuli was derived from a single full-length Mandarin Chinese song. The lyrics of this song were used consistently across all conditions and speakers.

One classification task was constructed by annotating the lyrics at syllable level:

- **Syllable Task**:  
  Based on syllable-level annotations using Praat. Used to train a separate dual-pathway CNN for syllable classification + genre classification.

This setup ensured controlled comparison of syllable-level decoding while preserving consistent acoustic and lexical content.

#### Annotation Procedure for Transfer Learning

- **Class Labels**:
  - For Syllable Task: 84-class Chinese syllable labels
  Labels were encoded into one-hot vectors using a `label_map.json` file.

#### Tools and Automation

- **Praat + Parselmouth**:  
  Used for manual alignment correction and boundary visualization.

- **Edge TTS Scripting**:  
  Automatically generated WAV files from textual prompts with boundary metadata.

- **NumPy Pipelines**:  
  Converted aligned audio into cochleagrams and stored them with synchronized metadata.

### Getting Started
(Core codes is pasted below)
1. Convert audio `.mp3` files in `data/music_audio/` to 16kHz `.wav`  
2. Generate cochleagrams using `cochleagram_generator.py`  
3. Train or evaluate CNN models with `TransferLearningCNN.py`  
4. Use label mappings in `data/label/` for classification outputs  
5. Explore feature pipelines via notebooks in `modules/Demo/`  

---

### Documentation

Refer to the `docs/` folder for:
- CNN architecture diagrams  
- Experimental setup  
- Data collection and annotation procedures  

---

## Acknowledgements

This project utilizes code from [mcdermottLab/kelletal2018](https://github.com/mcdermottLab/kelletal2018), developed by Kell et al. for their research published in *Neuron* (2018). We thank the authors for making their code available.

---

## Contact

For questions or contributions, feel free to open an issue or pull request.

---

**License**: [MIT](LICENSE)  
