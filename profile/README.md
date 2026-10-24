# HROCT Toolkit

The HROCT Toolkit is a local, cross-platform workflow for high-resolution OCT research data:

1. **HROCT Inference** segments OCT B-scans and creates Combined-QC confidence sidecars.
2. **Annotation Framework** reviews and corrects segmentation boundaries and foveal landmarks.
3. **ETDRS & QAF Analysis** measures regional layer thickness and optional raw Heidelberg `.vol` reflectivity, then creates individual or group thickness maps.

| Application | Repository | Windows | Linux |
| --- | --- | --- | --- |
| Segmentation | [HROCTInferenceApp](https://github.com/High-Resolution-OCT-Segmentation/HROCTInferenceApp) | CPU and NVIDIA GPU | NVIDIA GPU with CPU fallback |
| Review | [AnnotationFramework](https://github.com/High-Resolution-OCT-Segmentation/AnnotationFramework) | Yes | Yes |
| Quantitative analysis | [ETDRS_QAF_App](https://github.com/High-Resolution-OCT-Segmentation/ETDRS_QAF_App) | Yes | Yes |

Every application processes data locally. Standalone releases do not require Python. Developers and investigators who build from source can copy the exact commands from the project-specific README.

> Research software: these applications are not certified medical devices. Qualified investigators must review segmentation, foveal landmarks, QC coverage, and quantitative outputs before scientific or clinical interpretation.
