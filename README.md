# Canny Edge Detector

## Overview

The Canny Edge Detector is a multi-stage algorithm to detect a wide range of edges in images. It was developed by John F. Canny (1986) and remains a standard technique for edge detection because it balances detection quality, localization accuracy, and minimal response.

This README describes the algorithm, common parameters, usage patterns, implementation notes, and tips for tuning and performance.

## Key Features

* Multi-stage processing: smoothing, gradient calculation, non-maximum suppression, double thresholding, and edge tracking by hysteresis.
* Robust to noise (thanks to initial Gaussian smoothing).
* Produces thin, well-localized edges.
* Adjustable sensitivity via low/high thresholds and Gaussian sigma.

## Algorithm Steps (high level)

1. **Noise reduction**: Smooth the image with a Gaussian filter (controlled by sigma) to reduce noise.
2. **Gradient calculation**: Compute image gradients (magnitude and direction) using derivative filters (commonly Sobel).
3. **Non-maximum suppression**: Thin edges by keeping only local maxima along the gradient direction.
4. **Double thresholding**: Classify pixels as strong, weak, or non-relevant using high and low threshold values.
5. **Edge tracking by hysteresis**: Finalize edges by keeping weak pixels that are connected to strong pixels.

## Requirements

* An image processing environment (Python + OpenCV / scikit-image, MATLAB, C++ with OpenCV, etc.)
* Typical Python stack: numpy, scipy, opencv-python, scikit-image (optional)
* Input images: grayscale preferred (if color, convert to grayscale before processing)

## Key Parameters

* **sigma**: Standard deviation for the Gaussian smoothing kernel. Higher sigma → stronger smoothing → fewer false edges, but potential edge blurring. Common range: 0.5 — 3.0.
* **low_threshold**: Lower value for hysteresis thresholding (keeps weak edges connected to strong edges).
* **high_threshold**: Upper value for hysteresis thresholding (determines strong edges). Typically high_threshold > low_threshold.
* **kernel_size**: Size of Gaussian kernel (odd integer). Commonly derived from sigma (e.g., kernel = int(6*sigma + 1)).

## Usage Guidelines (conceptual)

* **Preprocessing**: Normalize or equalize contrast if images have poor lighting. Remove salt-and-pepper noise using median filtering before Canny if necessary.
* **Sigma tuning**: Use lower sigma to preserve fine details; increase sigma to suppress noise/artifacts.
* **Threshold selection**:

  * For automatic workflows, compute thresholds relative to image statistics (e.g., high = median * factor).
  * For fixed pipelines, tune on a representative dataset.
* **Scale and resolution**: For higher-resolution images, thresholds and sigma may need to be increased proportionally.
* **Color images**: Convert to grayscale (luminance) or process each channel and combine results (rarely needed).

## Common Pitfalls & How to Avoid Them

* **Too many noisy edges**: Increase sigma, tighten thresholds, or apply stronger denoising before Canny.
* **Broken or fragmented edges**: Lower the low_threshold or reduce sigma slightly; ensure hysteresis can link weak edges to strong edges.
* **Lost fine details**: Use smaller sigma and lower thresholds but accept more noise.
* **Parameter sensitivity**: Parameters that work for one image might fail on another—use adaptive thresholding or per-image heuristics when possible.

## Performance & Optimization Tips

* For real-time or large-batch processing: downscale image, run Canny, then upscale or map edges to original resolution if acceptable.
* Use separable Gaussian filtering for speed (many libraries do this automatically).
* When implementing in C/C++: leverage vectorized operations, OpenCV optimizations, and multi-threading.
* Consider GPU implementations (CUDA / OpenCL) for very large images or video streams.

## Evaluation & Metrics

* Evaluate edges qualitatively (visual inspection) and quantitatively when ground-truth edges exist:

  * Precision / Recall / F1 on edge pixels
  * Boundary localization error (distance between detected and true edges)
  * Computation time and memory usage

## References

* Canny, J. (1986). "A Computational Approach to Edge Detection." IEEE Transactions on Pattern Analysis and Machine Intelligence.
* OpenCV Canny documentation: search for "OpenCV Canny"
* scikit-image: `feature.canny` for a high-level Python implementation

## Contact / Attribution

For questions or to request example scripts (Python/OpenCV/Matlab), provide your preferred language and I can supply a compact sample.

---

