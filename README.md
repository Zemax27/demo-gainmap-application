# Apple HDR Gain Map Reconstruction

This Jupyter notebook reconstructs HDR images from Apple's SDR HEIC images and their associated gain maps.

## Quick Start on MyBinder

1. Go to https://mybinder.org/
2. Enter the GitHub repository URL
3. Click "launch"

## Technical Details

### The HDR Reconstruction Formula

The notebook implements Apple's gain map formula:

```
L_hdr = L_sdr × (1 + (Headroom - 1) × GainMap)
```

Where:
- `L_sdr` is the SDR (Standard Dynamic Range) image
- `GainMap` is the additional brightness information
- `Headroom` is extracted from XMP metadata (how much brighter HDR is vs SDR)

### Color Space Pipeline

1. **Input**: Display P3 color space (Apple devices)
2. **Linearization**: Remove gamma encoding
3. **Gain Application**: Apply the formula above
4. **Color Adaptation**: Convert to BT.2020 using Bradford chromatic adaptation
5. **PQ Encoding**: Apply Perceptual Quantizer transfer function
6. **Output**: 16-bit PNG with cICP metadata chunk

### Output Specifications

- **Format**: 16-bit PNG
- **Color Primaries**: BT.2020 (ITU-R Recommendation BT.2020)
- **Transfer Function**: PQ/ST.2084 (Perceptual Quantizer)
- **Reference White**: 203 nits
- **Peak Brightness**: 1000 nits
- **Metadata**: cICP chunk for HDR signaling

## Troubleshooting

### "Files not found" error
- Ensure you've created the `data/` folder
- Verify your files are named exactly as expected
- Check file extensions are correct (.HEIC, .png)

### Import errors
- On mybinder, dependencies should install automatically
- If running locally, use: `pip install -r requirements.txt`

### "No XMP metadata" warning
- The gain map PNG should contain XMP metadata with headroom value
- If missing, default headroom of 1.0 is used (no HDR enhancement)

## License

This notebook is provided for educational purposes.
