# Model Card

> **⚠️ RESEARCH USE ONLY:** This system is a collection of open-source models combined with proprietary technology and is intended exclusively for research purposes. It is NOT approved for clinical use. If you wish to adapt or deploy similar capabilities in your environment, please contact us for appropriate licensing and implementation options.

- **Model Version:** 2.0.0
- **Model Type:** Multi-component AI system combining generative models, analysis models, and report generation

## System Architecture

This system orchestrates four main AI components that work together to generate, edit, analyze, and report on chest X-ray images:

### 1. RadEdit Latent Diffusion Model (Image Generation)

text-to-image latent diffusion model is trained from scratch to generate chest X-rays from either the impression section of a radiology report (a short clinically actionable outline of the main findings) or a list of radiographic observations.

RadEdit is described in detail in RadEdit: stress-testing biomedical vision models via diffusion image editing (F. Pérez-García, S. Bond-Taylor, et al., 2024).

[RadEdit](https://huggingface.co/microsoft/radedit)

### 2. Maira-2 (Report Generation Model)

Processes generated or edited images to produce radiology-like reports.

Can incorporate additional fields like &ldquo;indication&rdquo; or &ldquo;comparison&rdquo; in its report generation.

### 3. Chest X-ray Analysis Model

Analyzes generated images to predict demographics (age, sex, view) and create anatomical segmentation masks.

## Intended Use

- **Primary Use:** Generating and analyzing synthetic medical images for research and development (R&D) or educational purposes.
- **Intended Users:** Researchers, healthcare professionals for non-clinical research, AI developers, academic institutions.
- **Out-of-scope Use Cases:** This system is NOT approved or intended for clinical use, diagnosis, treatment planning, or patient management. This system is a research tool/prototype and should not be used clinically.

## Training Data

- **Data Source:** [The primary data comes from the MIMIC-CXR-JPG dataset, accessible at PhysioNet](https://physionet.org/content/mimic-cxr-jpg/2.0.0/)
- **Data Description:** The dataset consists of radiography (X-ray) images with a resolution of 512x512 pixels.
- **Data Preprocessing:** The original images were resized to make the smallest sides have 512 pixels. When inputting it to the network, we center cropped the images to 512 x 512. The pixel intensity was normalised to be between [0, 1]. The text data was obtained from associated radiological reports. We randomly extracted sentences from the findings and impressions sections of the reports, having a maximum of 5 sentences and 77 tokens. The text was tokenised using the CLIPTokenizer from the transformers package (pretrained model 'stabilityai/stable-diffusion-2-1-base') and then encoded using CLIPTextModel from the same package and pretrained model.

## Licensing Information

This system combines multiple models with different license types:

- **Latent Diffusion Model:** Apache License 2.0
- **RadEdit and Maira-2:** Microsoft Research License - For research purposes only, not for commercial use.
- **Chest X-ray Analysis Model:** Creative Commons Attribution 4.0 International Public License

## Ethical Considerations

- **Bias and Fairness:** Efforts are made to ensure data diversity, but the system's bias and fairness depend on the representativeness of the training datasets of all component models.
- **Privacy:** All models are trained on de-identified data, ensuring compliance with GDPR and HIPAA regulations.

## Limitations

- **Generalizability:** The system's performance across different medical conditions or imaging modalities may vary and should be thoroughly tested before use in new contexts.
- **Robustness:** Known issues include potential degradation in performance under adversarial conditions or when exposed to out-of-distribution prompts.
- **Safety and Security:** System outputs should be reviewed by qualified professionals and are intended for research purposes only.
- **Composite System Limitations:** As this is a combination of multiple models, performance and behavior might vary across different components and their interactions.

## References

[1] Pinaya, Walter HL, et al. 'Brain imaging generation with latent diffusion models.' MICCAI Workshop on Deep Generative Models. Springer, Cham, 2022.

[2] Johnson, A., Lungren, M., Peng, Y., Lu, Z., Mark, R., Berkowitz, S., & Horng, S. (2019). MIMIC-CXR-JPG - chest radiographs with structured labels (version 2.0.0). PhysioNet. https://doi.org/10.13026/8360-t248.

[3] Johnson AE, Pollard TJ, Berkowitz S, Greenbaum NR, Lungren MP, Deng CY, Mark RG, Horng S. MIMIC-CXR: A large publicly available database of labeled chest radiographs. arXiv preprint arXiv:1901.07042. 2019 Jan 21.
