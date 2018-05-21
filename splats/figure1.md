---
layout: splat
title: Creating a gel electrophoresis figure with GIMP and Inkscape
---

This workshop will guide you through the creation of an annotated gel figure using two open-source programmes called *GIMP* and *Inkscape*.

Important buttons and features are highlighted on the images with red circles and font. File names and menu instructions are highlighted in a `different font`.

If you have any problems, please raise your arm and wait for a member of staff of demonstrator to help you out.

## Exercise file

* [Unprocessed gel electrophoresis image]({{ "/splats/images/figure1/gel.tif" | absolute_url }})

Depending on your system, clicking on the above link will download `gel.tif` automatically, or open the file in the browser (in which case you can `File > Save As the image`).

Copy `gel.tif` to a working directory (e.g. in your `Documents` folder). As you progress through the work, you will save different versions and other files to this folder.

## Labelling an electrophoresis gel

Open the programme `GIMP` (just hitting the windows key and typing “GIMP” should reveal it). To make your like with GIMP a little easier combining all panels through the menu option `Windows > Single-Window Mode`. Now drag and drop the file `gel.tif` onto the main window. Alternatively, the menu `File > Open` leads to the same result. 

![Drag and drop a file into GIMP]({{ "/splats/images/figure1/fig1.png" | absolute_url }})

Invert the image with through the menu `Colors > Invert` (inverting gel images is optional and down to personal preference).

The contrast and white levels of the gel can be adjusted through `Colors > Levels`. However, it is a good idea to first duplicate the layer on the layers-panel.

![Duplicate layer]({{ "/splats/images/figure1/fig2.png" | absolute_url }})

Then select the upper `gel.tif` copy to adjust contrast to taste. The histogram shows the distribution of pixel according to brightness, with black (0) being at the far left and white (255) being at the far right. The white triangle on right (or the number on the right) shifts the white point – turning more pixels white, while the black triangle on the left shifts the black point – turning more pixels black. The grey triangle in the middle changes the gamma of the image, which alters the gradients of how pixels change brightness.

<p class="message">For scientific publications it is considered very bad practise to alter the gamma, as this may falsify your data.</p>

![Adjust the black and white point in the histogram]({{ "/splats/images/figure1/fig3.png" | absolute_url }})

Adjusting levels is an irreversible process in `GIMP`. If you are unhappy with your results, delete the offending layer (select it and click the little `rubbish-bin icon`) and start anew. Do not readjust, as this may create banding or halo artefacts in your gel image.

Once you are happy with your settings, export the image as a `.tif` file via the menu `File > Export As...`. Use a descriptive name such as `gel_inverted_contrast.tif`.

<p class="message">Never overwrite original research images! Some journals even require them for authentication purposes.</p>

Open the programme `Inkscape`. Use `File > Import` and search for your `gel_inverted_contrast.tif`. You can either decide to embed the image in the file (leads to larger file size), or link it, in which case any edits done to the original will show in `Inkscape` as well (handy if you like to manipulate the source files, e.g. change the contrast).

![Embed of link a file in Inkscape]({{ "/splats/images/figure1/fig4.png" | absolute_url }})

On the main image stage, you can quickly navigate through drag & drop, either by holding and releasing the middle mouse button, or by holding down the space bar. Zoom in and out by holding down `Ctrl` and turning the `mouse wheel` ([more Inkscape keyboard shortcuts](https://inkscape.org/en/doc/keys045.html)).

Now that we have imported the electrophoresis image, we will want to crop it to appropriate proportions. Using the `rectangle tool` from the left toolbar (you can also press `F4`), click and drag a rectangle around the proportion of the gel you want to keep.

![Drag and click to draw a rectangle]({{ "/splats/images/figure1/fig5.png" | absolute_url }})

The `select tool` (`F1`) will allow you to resize this box by dragging and clicking on any of the handles (double sided arrows). A single click on the box with the select tool will change the rectangles handles to allow rotation (also works on the gel).

![Rectangles can be resized by the double-arrow handles]({{ "/splats/images/figure1/fig6.png" | absolute_url }})

After selecting both objects (`shift + click`), `Object > Clip > Set` will hide the gel parts outside the rectangle. You can always reverse the process through `Object > Clip > Release`.

![Setting a clipping mask]({{ "/splats/images/figure1/fig7.png" | absolute_url }})
![The clipping mask reversibly crops the gel image]({{ "/splats/images/figure1/fig8.png" | absolute_url }})
 
Next we’ll want to apply some subtle labels to the lanes using the `text tool` (`T` or `F8`). With the `select tool` (`F1`) click and drag your text to taste – in the example the labels are staggered for ease of reading. By selecting several at the same time (shift click or drag a selection rectangle) several labels can be aligned with each other, e.g. to their bottom edge in the right-hand panel. Make sure you align relative to the selection, not to the drawing.

![Typing and aligning text]({{ "/splats/images/figure1/fig9.png" | absolute_url }})

Precisely aligning and distributing labels is one of the major strengths of vector design programmes. Make sure to play with various settings to familiarise yourself.

Say the third band from the top is of particular interest. Let’s highlight its position with an arrow head. With the draw `freehand tool` (`F6`) click once to start drawing a line segment (or click and drag to draw any line shape). By holding down the `Ctrl` key you can restrict the line to specific angles (e.g. 0°, 30°, 45°, 60°, 90°, …). Click a second time to finish your line segment. Select it with the `selector tool` (`F1`) and in the `fill` and `stroke` panel under Markers select and arrowhead of your liking. Here you can also adjust its size by changing the stroke width.

![Create a line with an arrow head]({{ "/splats/images/figure1/fig10.png" | absolute_url }})

Next, with the `Node tool` (`F2`), select the arrow, then click onto the right node and click-drag (hold down `Ctrl` to keep your movement horizontal) the node until it is hidden by the arrowhead. With the `selector tool`, position the arrowhead next to the most right-hand 3rd band from the top (or whichever band you would like to point out). Alternatively, the stars and `polygon tool` (`*`) can create very simple triangles (corners set to 3!) to highlight information in your images.

![The line can be hidden within the arrowhead, or the polygon tool can create a triangle arrowhead]({{ "/splats/images/figure1/fig11.png" | absolute_url }})

What good is a marker without a few labels? Find the appropriate sizes from the [manual for the 1KB+ DNA ladder](https://tools.thermofisher.com/content/sfs/manuals/1Kb_Plus_DNA_ladder_man.pdf) used. 

In this example, it suffices to highlight a few bands at the extremes of the marker (to give the reader the full scale) and the bands surrounding our band of interest. Create short line segments (`Ctrl+D` duplicates a selection *in situ*) and text to highlight the bands of 12000, 1650, 1000 and 100bps. Use the alignment tools for consistent alignment.

Congratulations, you have professionally labelled your first electrophoresis gel! Now it is time to save (if you haven’t done so already) and export it an appropriate format. First, you may want to resize the canvas/drawing area to the size of your figure in the document properties – if you want to design your figure to a specific size, you may want to set this up at the beginning. Though the A4 default give you a good idea of how large your figure will look in a normal printed report or paper.

![Resize the drawing area]({{ "/splats/images/figure1/fig12.png" | absolute_url }})

`File > Export PNG Image...` will open up another side-panel-dialogue with settings for image size and exported area. The higher the dpi (dots per inch), the more the rasterised figure can be scaled up later – though this will also scale up your labels (in my case they were all 10pt).

![Exporting as PNG file]({{ "/splats/images/figure1/fig13.png" | absolute_url }})

Alternatively, you might want to `File > Save As...` your figure as PDF to preserve the vector properties of the labels. Most modern word processors can import PDF and maintain scalability. This is also useful if you decide to use `pdfLaTeX` to create documents.

![Exporting as PDF file]({{ "/splats/images/figure1/fig14.png" | absolute_url }})
 
## Finished example figure with legend

The legend is fictional, but should give you an idea of how much information you are expected to provide for a gel.

![Example of an annotated gel]({{ "/splats/images/figure1/gel_annotated.png" | absolute_url }})

<small>Figure 1: Agarose gel electrophoresis of BamHI/XhoI restriction endonuclease digestions of several ADORA1 mutants (amino acid substitutions above the lanes). The white arrowhead highlights the vector pcDNA3.1, while the black arrowhead highlights the excised ADORA1 band of approximately 1kb. Contaminating DNA in Y23A is shown by an asterisk.</small>
