# ellipsefits

This is Python code for reading, processing, and plotting the isophotal
ellipse fits generated by the IRAF/STSDAS "ellipse" task (part of the
stsdas.analysis.isophote package).

## Examples of use:

First, generate ellipse-fit output using the IRAF task ellipse, then convert
the output to either FITS table format or text-file format using the
tcopy or tdump tasks. The included IRAF script doellipse.cl will automatically
do both. E.g., to fit an object in the image myimage.fits with initial guesses
for the center of (x,y) = (512.5, 510.3), initial semi-major axis of 100 pixels,
initial ellipse position angle = 45 degrees and ellipticity = 0.3, and
maximum semi-major axis = 300 pixels:

    cl> doellipse myimage.fits el_myimage 512.5 510.3 100 45 0.5 300

This will generate three output files: el\_myimage.tab (STSDAS TABLES format),
el\_myimage\_tdump.txt (text table), and el\_myimage.fits (FITS table).

Then, in Python:

    >>> import ellipsefit
    >>> efit = ellipsefit.ReadEllipse("/path/to/el_myimage.fits")
    >>> ellipsefit.PlotEllPA(efit)

The same, but specifying the pixel scale of the image (here, 0.396
arcsec/pixel) and its orientation on the sky, so that plots will display
semi-major axis in arc seconds and position angle on the sky; the plotting
command specifies log spacing on the x-axis, a restricted x-axis range,
and a restricted y-axis range for the position-angle plot:

    >>> efit = ellipsefit.ReadEllipse("/path/to/el_myimage.fits", pix=0.396, telPA=85.7)
    >>> ellipsefit.PlotEllPA(efit, xlog=True, xrange=[10,100], parange=[10,30])



## Requirements:
This should work under any recent version of Python 3; it also works
in Python 2.7.

Required Python libraries:

   * numpy
   * scipy
   * matplotlib
   * astropy


It *cannot* read the standard (STSDAS TABLES) output table files generated by
"ellipse"; these tables should be converted to FITS tables using "tcopy" or to
text file using "tdump" (both of which are tasks in the STSDAS TABLES package).
The included IRAF script "doellipse.cl", which is a wrapper around the "ellipse" task,
will automatically generate both conversions.

## License

This code is released under a standard 3-clause BSD license.