# Dental Gap Detector

## Overview
This MATLAB script detects gaps between upper and lower teeth, as well as between individual teeth, in dental radiographs. It implements a series of image processing techniques to enhance contrast, detect dark regions, and approximate the detected gaps with lines.

## Usage

### Requirements
- MATLAB

### Instructions
1. Clone or download this repository to your local machine.
2. Place your dental radiograph image (e.g., `teeth_sample.png`) in the same directory as the script.
3. Open MATLAB and navigate to the directory containing the script.
4. Run the script `dental_gap_detector.m`.

```matlab
% Example usage
run dental_gap_detector.m
```

## Functionality

- Loads the dental radiograph image.
- Enhances contrast using adaptive histogram equalization.
- Divides the enhanced image into vertical sections.
- Detects dark regions within each section.
- Approximates gaps between upper and lower teeth with quadratic curves.
- Identifies gaps between teeth using perpendicular lines.
- Overlays the detected lines on the original image.

## Parameters
- `num_stripes`: Number of vertical sections to divide the image.
- `upper_jaw_step_size`: Step size for upper jaw line detection.
- `lower_jaw_step_size`: Step size for lower jaw line detection.
- `line_length`: Length of perpendicular lines.
- `intensity_threshold`: Intensity threshold for lower jaw line detection.
- `intensity_threshold1`: Intensity threshold for upper jaw line detection.

## Output
The script displays the original dental radiograph with the calculated lines superimposed on the image.

## Acknowledgments
- The script is inspired by the work described in (Jain and Chen, 2004).
- Reference: Jain, A.K., and Chen, H. (2004). Matching of dental X-ray images for human identification. Pattern Recognition, 37:1519-1539.

