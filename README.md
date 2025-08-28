# Student internship after the 2nd year: speckle interferometry.

## Cell 1: master-bias calculation

In CMOS detectors, a certain constant value is added to the signal, which is called the bias.
This value is fairly stable, but it still depends on the position on the detector and on time.
To analyze the stability of the bias, several series of dark frames (frames without illumination) are taken. In this case, it is 1000 frames.
From the entire set of frames, a pixel-by-pixel median is calculated. This result is the master bias:

<image src="/master_bias.png">

## Cell 2: obtaining the image power spectrum.

To begin with, the master bias is subtracted from the raw telescope frame.
Next, the region containing the target object is cropped.
For this, take the gauss_filter from the SciPy library.
Then the brightest pixel is found, and a 100 × 100 pixel region is cropped around it.
After that, the mean background level must be determined: using areas of the frame outside the target window, the average background was estimated and subtracted from the entire frame.
To obtain the image power spectrum, a Fourier transform was applied to each frame individually, followed by averaging; for convenience, corrupted frames were discarded.

<image src="/log_powerspectrum_cal334.png">

## Cells 3 and 4: side effects reduction

A circular mask is applied to the power spectrum here. The principle of its application is as follows: outside this mask, there should be no signal, but there is.
It is also necessary to perform azimuthal averaging: a blank 100 × 100 frame was created and, by rotating the power spectrum by 1°, it was accumulated into this blank frame. Then, the resulting frame was divided by 360.

<image src="/masked_powerspectrum_cal334.png">

## Cells 5 and 6: model selection and refinement.

The resulting power spectrum is approximated by a certain model function.
The initial model is selected manually and looks like a set of parallel, alternating light and dark stripes (see cell 5).
Then this model is fitted to the true spectrum by the computer. To do this, the region of the spectrum is selected (see cell 4) where the stripes are present.

<table><tr>
<td>Selected <img src="model_cal334.png" alt="Drawing" style="width: 500px;"/> </td>
<td>Optimized <img src="optimized_model_cal334.png" alt="Drawing" style="width: 500px;"/> </td>
</tr></table>

The final stage of the work was the concatenation of the optimized model and the resulting power spectrum, followed by visual comparison.

<image src="/concatenated_model_and_reality_cal334.png">
