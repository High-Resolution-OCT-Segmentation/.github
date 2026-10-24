# High Resolution OCT Segmentation Workflow

The High Resolution OCT Segmentation Workflow is a local, cross-platform pipeline for segmenting, reviewing, correcting, and quantitatively analyzing high-resolution OCT volumes.

## Workflow

### 1. HROCT Inference

**HROCT Inference** recursively discovers OCT volumes below a selected parent folder and segments their B-scans. For every processed image, it stores the segmentation boundaries and the Combined-QC confidence information beside the original image. The source images remain in their existing folders.

### 2. Annotation Framework — optional but recommended

**Annotation Framework** opens the same image folders produced by the inference step. It automatically loads each image together with its segmentation file and, when available, its Combined-QC confidence file. Investigators can inspect the Combined-QC support map and manually correct segmentation boundaries or foveal landmarks. Saved corrections remain beside the corresponding source image in the format used by the other applications.

Images without segmentation or confidence files can still be opened. In that case, no segmentation or Combined-QC overlay is shown until the corresponding data are created.

### 3. ETDRS & QAF Analysis

**ETDRS & QAF Analysis** reads the current segmentation files directly from the OCT volume folders. These may be the original results from HROCT Inference or the manually corrected results saved by Annotation Framework. The application validates the available scans, supports volumes containing 121 or 241 B-scans plus one square en-face image, determines or imports the foveal position, and calculates regional layer-thickness results and individual or group thickness maps.

Raw Heidelberg `.vol` data are optional. If a matching `.vol` file is available, raw reflectivity analysis can be added; without one, the complete thickness analysis remains available. Missing or corrupted segmented scans are skipped and reported through coverage information rather than being replaced with invented measurements.

## Data flow

```text
OCT image folders
        |
        v
1. HROCT Inference
   - segmentation boundaries beside every processed B-scan
   - Combined-QC confidence data beside every processed B-scan
        |
        +------------------------------+
        |                              |
        v                              v
2. Annotation Framework           3. ETDRS & QAF Analysis
   - visual quality control           - direct analysis of inference output
   - manual corrections               - no manual correction step
        |
        v
3. ETDRS & QAF Analysis
   - analysis of reviewed and corrected segmentation data
```

The recommended research workflow is **HROCT Inference → Annotation Framework → ETDRS & QAF Analysis**. For automated processing, inference results can also be passed directly from **HROCT Inference → ETDRS & QAF Analysis**. No separate export is required: the applications exchange data through the files stored alongside the OCT images.

| Step | Application | Repository | Windows | Linux |
| --- | --- | --- | --- | --- |
| 1. Segmentation | HROCT Inference | [HROCTInferenceApp](https://github.com/High-Resolution-OCT-Segmentation/HROCTInferenceApp) | CPU and NVIDIA GPU | NVIDIA GPU with CPU fallback |
| 2. Review and correction | Annotation Framework | [AnnotationFramework](https://github.com/High-Resolution-OCT-Segmentation/AnnotationFramework) | Yes | Yes |
| 3. Quantitative analysis | ETDRS & QAF Analysis | [ETDRS_QAF_App](https://github.com/High-Resolution-OCT-Segmentation/ETDRS_QAF_App) | Yes | Yes |

Every application processes research data locally. Standalone releases do not require Python. Developers and investigators building from source can copy the exact setup and build commands from each project-specific README.

> **Research software:** These applications are not certified medical devices. Qualified investigators must review segmentation boundaries, foveal landmarks, QC coverage, and quantitative outputs before scientific or clinical interpretation.
