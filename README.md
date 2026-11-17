# Music EEG

This project explores the neural channel capacity of the human brain in processing music and speech using EEG data and a task-optimized, interpretable CNN.

Inspired by previous studies in auditory cognition and information theory, we investigate how the brain allocates resources when decoding musical genres versus linguistic content under varying playback conditions. The framework integrates EEG-based experiments, linguistic feature analysis, and a dual-pathway CNN model to simulate auditory perception and establish parallels between biological and computational processing limits.

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
