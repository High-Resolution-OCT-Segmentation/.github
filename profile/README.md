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

| Step | Application | Repository | Windows | Linux | macOS |
| --- | --- | --- | --- | --- | --- |
| 1. Segmentation | HROCT Inference | [HROCTInferenceApp](https://github.com/High-Resolution-OCT-Segmentation/HROCTInferenceApp) | CPU and NVIDIA GPU build | NVIDIA GPU build with CPU fallback | No standalone build; experimental CPU source use only |
| 2. Review and correction | Annotation Framework | [AnnotationFramework](https://github.com/High-Resolution-OCT-Segmentation/AnnotationFramework) | Standalone build | Standalone build | No standalone build; experimental source use only |
| 3. Quantitative analysis | ETDRS & QAF Analysis | [ETDRS_QAF_App](https://github.com/High-Resolution-OCT-Segmentation/ETDRS_QAF_App) | Standalone build | Standalone build | No standalone build; experimental source use only |

## Building from source: first-time setup

Supported standalone build workflows exist for Windows and Ubuntu Linux. There is currently no macOS standalone build. The instructions below explain how to clone a repository and create the Windows or Linux application locally. Python and Git are required on the build computer. The generated file in the documented `dist` or `dist-linux` folder contains the Python runtime and can then be copied to a compatible target computer.

Use a 64-bit computer, an internet connection, and at least 10 GB of free disk space. Python 3.11 is the shared recommended version. Each application is installed into its own virtual environment, so it does not modify other Python projects.

### Windows 10 or 11

1. Download and install the appropriate [Python 3.11.9 installer from Python.org](https://www.python.org/downloads/release/python-3119/) (normally **Windows installer (64-bit)**). Enable **Add Python to PATH** if the installer offers that option.
2. Download and install [Git for Windows](https://git-scm.com/install/windows) with its default options.
3. Close the installers. Open the Windows Start menu, type **PowerShell**, and open **Windows PowerShell**.
4. Paste these verification commands:

```powershell
py -3.11 --version
git --version
```

Both commands must print a version. Do not continue if either command says that it is not recognized; close and reopen PowerShell first, then repair the corresponding installation if necessary.

### Ubuntu or Debian Linux

1. Open **Terminal** from the application menu or press `Ctrl+Alt+T`.
2. Paste:

```bash
sudo apt update
sudo apt install -y git python3 python3-venv python3-pip build-essential
python3 --version
git --version
```

The Linux build scripts create their own virtual environments and install the remaining dependencies automatically. A graphical Linux desktop and the Qt/XCB runtime libraries listed in the relevant project README are required to launch the applications.

### macOS

1. Download and install the [Python 3.11.9 macOS 64-bit universal2 installer from Python.org](https://www.python.org/downloads/release/python-3119/).
2. Press `Command+Space`, type **Terminal**, and press Return.
3. Install Apple's command-line tools, which include Git:

```bash
xcode-select --install
```

4. After the installer finishes, close and reopen Terminal, then verify:

```bash
python3 --version
git --version
```

macOS can be used for source development, but the current reproducible one-file release builds are validated only on Windows and Ubuntu Linux. A macOS application must be built and tested on a Mac; PyInstaller cannot create Windows, Linux, and macOS applications from one operating system.

### Clone and build an application

Choose one application below and follow its project-specific instructions. The commands create a local folder, an isolated Python environment, install dependencies, run tests where provided, and identify the finished file inside the project's `dist` or `dist-linux` folder.

- [Build HROCT Inference](https://github.com/High-Resolution-OCT-Segmentation/HROCTInferenceApp#running-from-source)
- [Build Annotation Framework](https://github.com/High-Resolution-OCT-Segmentation/AnnotationFramework#run-from-source)
- [Build ETDRS & QAF Analysis](https://github.com/High-Resolution-OCT-Segmentation/ETDRS_QAF_App#running-from-source)

Build on the target operating system: Windows builds must be created on Windows, Linux builds on Linux or WSL2, and macOS builds on macOS. Copy the commands exactly and wait for each command to finish before pasting the next one.

Every application processes research data locally. Project-specific requirements, launch commands, build commands, and output locations are documented in the linked README.

> **Research software:** These applications are not certified medical devices. Qualified investigators must review segmentation boundaries, foveal landmarks, QC coverage, and quantitative outputs before scientific or clinical interpretation.

## Citation

If any application or any part of this workflow is used for research, analysis, figures, derived data, or a publication, the associated High Resolution OCT Segmentation Workflow paper **must be cited**.

> **Citation pending:** The paper is currently under review. The complete citation and BibTeX entry will be added here after acceptance. Until then, record the repository URL, application version or commit, and access date so the exact software version remains traceable.
