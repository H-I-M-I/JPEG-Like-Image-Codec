# Lab 3 - Quantization and a JPEG-like Image Codec

[Open the notebook](DT096A_Lab3.ipynb) · [Browse exported results](results/)

This lab takes compression further than a visual quantization comparison. It measures distortion and estimated rate, builds **actual decodable compressed files**, and checks how different quantization choices affect reconstructed images. The tests use grayscale versions of the `camera` and `astronaut` images.

## Section 3 - Scalar quantization

Uniform **midpoint reconstruction** quantization is tested at different bit depths. Output images, error maps and intensity histograms are saved. Since reconstructed values are the midpoint of each bin, the output convention is intentionally different from a quantizer that reconstructs at bin endpoints.

![Scalar quantization comparison](results/figures/S3_camera_quantization.png)

## Section 4 - Block DCT and quantization

Each image is padded to an 8 × 8 block boundary, shifted by 128, transformed with an orthonormal **2D DCT**, quantized and reconstructed with an inverse DCT. The tests compare a frequency-weighted quantization matrix with alternative matrices, including flat and gentler/stronger settings.

![DCT transform comparison](results/figures/S4_camera_transform.png)

The DCT alone is approximately reversible; **quantization** is the stage that intentionally discards information. The experiments make that difference visible in both error images and coefficient/energy plots.

## Sections 5-6 - Rate, distortion and image quality

Reconstruction quality is evaluated using **MSE, PSNR and SSIM**. Entropy of quantized DCT coefficients gives an **estimated bits-per-pixel (bpp)** measurement. Rate-distortion curves compare different quantization strengths.

![Rate-distortion curves](results/figures/S5_S6_rate_distortion_curves.png)

**Important detail:** estimated bpp is not the same as the actual `.ljc` file size. The estimate leaves out the complete header and model-storage overhead; actual file sizes are recorded separately in Section 7.

### Blind visual comparison

The notebook also prepares a four-image blind comparison. Two quantization patterns are searched for near **30 dB PSNR**, but a similar PSNR does not guarantee that the artifacts will look the same. The folder contains comparison images and a blank participant-ranking template.

**No participant rankings are claimed here.** The uploaded ranking template is blank, and the private condition key has been kept out of this public package.

## Section 7 - A small self-contained codec

The codec uses an 8 × 8 DCT and quantized coefficients, then compares:

1. Direct Huffman coding of coefficients
2. Zigzag ordering + run-length encoding (RLE) + Huffman coding

Generated `.ljc` files include the information needed by the custom decoder: dimensions, quantization matrix, symbol frequencies and bit-packed data. Decoding is verified in the notebook, and measured actual bytes and bpp are recorded.

![Actual codec rate-distortion comparison](results/figures/S7_actual_codec_rate_distortion.png)

These are **custom JPEG-like files**, not `.jpg` files and not a complete implementation of the JPEG standard. In particular, this experiment does not implement JPEG's byte syntax or DC prediction.

## Results folder

- `results/figures/` - comparison layouts, histograms, energy maps and rate-distortion plots
- `results/images/` - processed originals and reconstructed PNGs
- `results/tables/` - metrics, codec measurements, quantization matrices and source records
- `results/bitstreams/` - encoded custom `.ljc` files
- `results/subjective/` - blind comparison images and blank ranking template

The private answer key is deliberately omitted to avoid revealing which visual condition is which. An additional source backup should be kept privately if the blind study is still ongoing.

## Running the notebook

Open the notebook in Colab or Jupyter and run in order. Dependencies include NumPy, Matplotlib, Pillow, SciPy, scikit-image, pandas and IPython. Existing exported results are included for browsing, so running the full codec is not necessary just to inspect the figures.
