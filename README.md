# APi-reg
Image registration processes for all-purpose images (non-specific and non-specialized)

```
    ___    ____  _                      
   /   |  / __ \(_)     ________  ____ _
  / /| | / /_/ / /_____/ ___/ _ \/ __ `/
 / ___ |/ ____/ /_____/ /  /  __/ /_/ / 
/_/  |_/_/   /_/     /_/   \___/\__, /  
                               /____/
```  
                               
# Introduction
This repo is the result of a research for the Information Design course at [UNIRSM](https://design.unirsm.sm/) (2024), in collaboration with the [Visual Contagions project](https://www.unige.ch/visualcontagions/) from the [University of Geneva](https://www.unige.ch/).
The goal of the research is to find the best ways and practices to automatically register (align) a stack of images that contain the same picture, like the following:
</br>
<br>
<img align="left" width="20%vw" height="auto" src="https://github.com/giaaaacomo/APi-reg/assets/38686675/b863c30f-3f66-4192-9d1a-8ceaa9acf523">
<img align="left" width="20%vw" height="auto" src="https://github.com/giaaaacomo/APi-reg/assets/38686675/8836a3f9-562d-4481-a886-c4b2f1b5f832">
<img align="left" width="20%vw" height="auto" src="https://github.com/giaaaacomo/APi-reg/assets/38686675/c919fcf2-bec8-4fe3-adbb-6d333e4c70eb">
<img align="left" width="20%vw" height="auto" src="https://github.com/giaaaacomo/APi-reg/assets/38686675/9659f2f1-2849-4503-bf86-a8879f201721">
<br clear="left"/>

>
>Initially, the goal was to create a visualization tool that allowed to perceive the variation in space and time of the layouts containing different images of the same artwork in context (eg.different scans of the Monnalisa from different publications), from the Visual Contagions artwork cluster. Since the initial prompt, the interest regarding the technological aspects about the automation of the process took the priority over the tool itself, shifting the project to this research.

# Requirements
* Image files of the same format and size (although not necessary for all the solutions, it's a good way to be sure to be able to experiment with different tools). This can easily be done by creating a blank file on Photoshop with the desired size, importing all the photos as different layers and exporting every layer as a single image

# Hardware used for the tests:
MSI GP76 Leopard
* Intel core i7-11800H (8 core, 16 threads)
* NVidia RTX 3070 8gb
* 16gb RAM

ASUS ROG GL502VM
* Intel core i7-7700 (4 cores, 8 threads)
* NVidia GTX 1060 6gb
* 40gb RAM

# Steps we took
## Programs tryout 
   
### * Photoshop (24.4.1)
  
   Photoshop allows to autoalign layers based on common features, following different patterns (automatic, perspective, collage, cylindrical, spherical, reposition)
   

   **review**
   > **pros:** user Friendly, works well with similar images, fast  
   > **cons:** unable to align correcly images with different structure
   


### * FIJI

  FIJI is an image analysis tool mostly used by bioanalysis. It's a combination of ImageJ and ImageJ2, and comes with many plugins preinstalled and an update manager that allows also to install new plugins within the app.

Trying FIJI means trying the different plugins that do image registration. [Here](https://imagej.net/list-of-extensions) is the official list, that can be filtered to show only registration plugins; from that list we selected those who seemed to fit the best: keep in mind that the original use case scenario for the software is biomedical analysis, therefore many of the available tools are tailor-made for cellular-scale elements, for applications like the study of cellular reproduction, bone healing, tumoral cells development and propagation. This means that the available tools more often than not won't work as if they were designed for all-purpose images in mind, and this also means that not all plugins will work with our images, depending on their diversity, resolution, and visual features. Keep also in mind that biomedical analysis workstations have powerful hardware, and the programs they use are though to be run in those machines.  
All of this leads to a steep selection in the available plugins (that will be explained further in the next chapter) and we ended up picking the following:
1. [Elastic Stack Alignment](https://imagej.net/plugins/elastic-alignment-and-montage)
2. [Descriptor-based series registration](https://imagej.net/plugins/descriptor-based-registration-2d-3d)
3. [Rigid Registration]
4. [Linear Stack Alignment with SIFT](https://imagej.net/plugins/linear-stack-alignment-with-sift)
5. [Linear Stack Alignment with SIFT/Multichannel](https://imagej.net/plugins/linear-stack-alignment-with-sift)
6. [Register Virtual Stack Slices](https://imagej.net/plugins/register-virtual-stack-slices)
7. [TurboReg](https://bigwww.epfl.ch/thevenaz/turboreg/)
8. [MultiStackReg](https://github.com/miura/MultiStackRegistration)
9. [N-D Sequence Registration](https://github.com/tischi/fiji-plugin-imageRegistration)
10. [Multiview-Reconstruction](https://imagej.net/plugins/multiview-reconstruction)
11. [DS4H Image Alignment](https://imagej.net/plugins/ds4h-image-alignment)
12. [Fijiyama](https://imagej.net/plugins/fijiyama)
13. [Template Matching and Slice Alignment](https://sites.google.com/site/qingzongtseng/template-matching-ij-plugin)
    
  **review**
  > **pros:** tons of options, super precise, super versatile, super customizable  
  > **cons:** tons of options, hardware intensive (especially CPU and RAM), few plugins support GPU acceleration, (therefore) slower


### * Hugin

Hugin focuses on the creation of large panoramas and mosaic of images. Despite this, its feature-recognizing algorithms allow to match images in stacks. The program allows to choose the level of complexity of the tools by going to ```interface``` and checking ```expert```.
The program allows to run and automate complex operation through terminal, but has an advanced GUI that allows for precision works.
 **review**
  > **pros:** allows for really precise operation, versatile GUI, tools are not too difficult to use, interesting result
  > **cons:** image stacking not working in the updated program, kinda steep learning curve, settings are confusing, advanced user oriented

### Other interesting tools still to try
* opencv online
* elastix

## In-depth process
### Photoshop
      1. import all the images as different layers
      2. rasterize every layer
      3. head to edit > auto-align layers
      4. choose one of the following patterns: automatic, collage (rotation, scale, translation), repositioning (translation)
         N.B. the choice of the pattern should follow the type of images. While automatic seems to be the best, collage should be more precise.  
      6. press ok
   
### FIJI

    1. import all the images as a serie (file > import > image sequence)
    2. go to plugin, head to registration, select the desired plugin

##### How do registration works (Feature extraction)
The basic concept followed by the majority of plugins used for this research is that the program first of all elaborates every image, looking for peculiar and recurring features, then compares every feature to the other images, then tries to match the features in every image by moving, rotating and scaling them. Some specific plugin use peculiar manipulations that work in custom ways (eg. Elastic Stack Manipolation).
There are two main methods for feature extraction: SIFT and MOPS. To summarize, here are the differences:
* SIFT: It detects keypoints in an image that are stable under changes like scale, rotation, and illumination. It uses a complex process involving finding extrema in a scale-space representation of the image and assigning orientations to these keypoints. Then, a descriptor is computed around each keypoint to capture the local image information.
  >*tends to be more precise, but also more complex to compute  
  >*works better even in challenging conditions 
* MOPS: It focuses on extracting oriented patches at different scales from the image. These patches are centered around interest points, typically corners detected using the Harris corner detector. The patch is then rotated to align with a dominant local gradient direction and a descriptor is obtained by sampling the intensity values within the patch.
  >*tends to be faster, both to compute and to compare  
  >*less precise  
  >*better distribution along the space  

##### Registration techniques:
There are different ways to register images, and the main are:  
* Translation  
* Rigid (translation + rotation)  
* Similarity (translation + rotation + isotropic scaling)  
* Affine  
* Elastic (via BUnwarpJ with cubic B-splines)  
(source: https://imagej.net/imaging/registration)

#### FIJI PLUGINS
  **disclaimer**
  >If you are using a computer with many CPU cores, you need to make sure that you have enough memory (RAM) to handle your images for alignment. In general, you'll need 10x the file size of your largest 2D image for each core (see: https://imagej.net/plugins/trakem2/#how-much-ram-should-i-allocate-to-the-jvm-for-fiji-to-run-trakem2). EXAMPLE: If your dataset consists of images that are 600MB each and your computer has 12 cores, you will need at least 72 GB of RAM (= 600 MB x 10 x 12 cores).  
cit. The University of Texas at Austin (Texas Advanced Computing Center)
 
 >>in our tests we used 32 images about ~3mb each, that ended up using up to 96% of the 16gbs of ram. Often, ram saturation ends up prematurely the process giving an error. The disclaimer above was written for the elastic alignment but should be followed as a rule of thumb for all the plugins. 

___
1. [Elastic Stack Alignment](https://imagej.net/plugins/elastic-alignment-and-montage)
   >*results:* failure  
   >*elapsed time:* ~12h (MSI)
   
   Subdivides the images into meshes of triangles; tries to connect the vertices simulating a spring-like deformation, then tries to match the content of the triangles and the vertices.
   Despite a lot of customizability, after 12 hours of processing the output that ESA gave us was a stack of 202 black images.
   
3. [Descriptor-based series registration](https://imagej.net/plugins/descriptor-based-registration-2d-3d)
   >*results:* X  
   >*elapsed time:* X
   new scheduling 

   Probably the most interesting plugin as it allows to select a partition of the first image and use that as a descriptor (basically checks only for the feats inside the selected area), but appears to be way slower than ESA; it also allows to adjust the threshold of the feats, thus adjusting the density of feats in the area.
   We had to re-schedule this test due to the time-intensive elaboration.

7. [Linear Stack Alignment with SIFT](https://imagej.net/plugins/linear-stack-alignment-with-sift)
   >*results:* failure  
   >*elapsed time:* ~15min (MSI)
   
   The plugin basically searches for SIFT features and tries to match them between images.
   It is actually able to match similar images, but struggles with more articulated ones.
   Tried with both affinity and similarity algorithms, the former gave better results, but was not able to keep a consistent position of the picture.  

    <div style="display: flex; flex-wrap: wrap;">
   <img src="https://github.com/giaaaacomo/APi-reg/blob/main/assets/linearstackalignmentsiftaffinity-ezgif.com-optimize.gif" width="250px" alt="text-1"><figcaption>LSA Affinity</figcaption>
   <img src="https://github.com/giaaaacomo/APi-reg/blob/main/assets/sift_similarity-ezgif.com-optimize.gif" width="250px" alt="text-2"><figcaption>MSR Rigid Body</figcaption>
</div>

9. [Linear Stack Alignment with SIFT/Multichannel](https://imagej.net/plugins/linear-stack-alignment-with-sift)
    >*results:* mixed  
    >*elapsed time:* ~25min (MSI)

    This is an advanced version of Linear Stack Alignment with SIFT, that is able to match the alignment to the desired channel.
   The results are mixed: worse than the basic LSA, but surprisingly good with another set of images.
   It requires the PTBIOP update from the update manager.
   <div style="display: flex; flex-wrap: wrap;">
   <img src="https://github.com/giaaaacomo/APi-reg/blob/main/assets/monalisa_multichannel_compressed.gif" width="250px" alt="text-1"><figcaption>LSA Monnalisa Multichanneled</figcaption>
   <img src="https://github.com/giaaaacomo/APi-reg/blob/main/assets/Aligned_deyck_livelli-ezgif.com-optimize.gif" width="250px" alt="text-2"><figcaption>LSA D'Eyck Multichanneled</figcaption>
</div>
  

11. [Register Virtual Stack Slices](https://imagej.net/plugins/register-virtual-stack-slices)
    >*results:* failure  
    >*elapsed time:*  ~20min (MSI)

    Seemed promising, but keeps reporting errors that abort the operation early. Looking online for solutions it seems that encountering a picture without recognizable features will stop the registration, making this tool ultra-sensible to image variations. 

13. [TurboReg](https://bigwww.epfl.ch/thevenaz/turboreg/)
    >~~*results:*~~  
    >~~*elapsed time:*~~  
    Used MultiStackReg instead, that is an advanced version of this.

15. [MultiStackReg](https://github.com/miura/MultiStackRegistration)
    >*results:* failure  
    >*elapsed time:* ~5 min (ROG)  

    MultiStackReg was made to align unregistered color channels in microscope pictures (when this happens, images get blurry). It's based on TurboReg, and above optimizations, adds the ability to check stacks of images instead of singular images.  
    Since it's purpose is to align channels of the same image, but we need it's ability to align different images, we need to pick one as a source, clone that as many times as there are images to compute, open the source (that now is a sequence) separately, then open the others as a sequence.  
    Since it's made to align channels, the images in both sequences need to have them splitted:  
        ```image>color>split channels```  
    Now we have to run the plugin and select the source as first image, using it as reference, the target as second image that needs to be aligned to the first stack. Since it allows the registration of only two channels at a time, it's possibile to do a second registration using the first as reference, to register the third channel. It is then possible to merge the channels back into one to get the colored images back.
    We tried both rigid body and affine methods, but we didn't get good results.
    <div style="display: flex; flex-wrap: wrap;">
    <img src="https://github.com/giaaaacomo/APi-reg/blob/main/assets/monalisa%20affineMSR.gif" width="250px" alt="text-1"><figcaption>MSR Affine</figcaption>
    <img src="https://github.com/giaaaacomo/APi-reg/blob/main/assets/monalisarigidbodyMSR-ezgif.com-optimize.gif" width="250px" alt="text-2"><figcaption>MSR Rigid Body</figcaption>
    </div>
   
17. [N-D Sequence Registration](https://github.com/tischi/fiji-plugin-imageRegistration)  
    ~~>*results:*~~   
    ~~>*elapsed time:*~~  
    not available anymore  
    

5. [Rigid Registration]
   >*results:* failure  
   >*elapsed time:* 5min (MSI)

   Simple tool made to correct color shift and translation; like MultiStackReg requires a single channel, but is able to apply the transformation to the other channels automatically. Seems to be unable to elaborate complex and diverse data such as the ones provided in the research.

19. [Multiview-Reconstruction](https://imagej.net/plugins/multiview-reconstruction)
    >*results:*queued   
    >*elapsed time:*  

21. [DS4H Image Alignment](https://imagej.net/plugins/ds4h-image-alignment)
    >*results:*queued   
    >*elapsed time:*  

23. [Fijiyama](https://imagej.net/plugins/fijiyama)
    >*results:*queued   
    >*elapsed time:*  
    scheduled -- needs an older java version
24. [Template Matching and Slice Alignment](https://sites.google.com/site/qingzongtseng/template-matching-ij-plugin)
    >*results:*confused   
    >*elapsed time:* ~5min (MSI)

    Uses opencv to elaborate the images; needs a specific server to be added to the server list in order to be downloaded into FIJI (https://sites.imagej.net/Template_Matching/). Differently from the other plugins, it is launched through ```Plugins>Template Matching>Align slices in stack...```. Like MultiStackReg needs a grayscale, therefore it's needed a channel split for the image serie. Similarly to the Descriptor-based series registration plugin, it allows to select an area of the image that needs to be searched in the other images. It also allows to search in a defined (by the user) area around the selected area. The output prints the displacement of the picture between the images, aligns the pictures to the area of the selected images, but doesn't seem able to resize the picture to match the other. Also doesn't seem to be very precise. Contacting the developer as this seems to be a possible solution if a workaround is found.

    <div style="display: flex; flex-wrap: wrap;">
    <img src="https://github.com/giaaaacomo/APi-reg/blob/main/assets/monalisa_tempmatchandslicealign.gif" width="250px" alt="text-1"><figcaption>Monalisa TMSA</figcaption>
  
</div>

##### Testing queued
26. [DS4H Image Alignment](https://imagej.net/plugins/ds4h-image-alignment)
27. [Multiview-Reconstruction](https://imagej.net/plugins/multiview-reconstruction)    
28. [Parallel-Fiji-CMTK-Registration](https://github.com/sandorbx/Parallel-Fiji-CMTK-Registration)
29. [Fijiyama](https://imagej.net/plugins/fijiyama)
30. [Descriptor-based series registration](https://imagej.net/plugins/descriptor-based-registration-2d-3d)
### Hugin
Hugin focuses on the creation of large panoramas and mosaic of images. Despite this, its feature-recognizing algorithms allow to match images in stacks. The program allows to choose the level of complexity of the tools by going to ```interface``` and checking ```expert```.
The program allows to run and automate complex operation through terminal, but has an advanced GUI that allows for precision works.
We focused on two methods:

#### Focus stacking with Hugin and Enfuse
After loading the images in Hugin, the program allows the selection of the (hypothetical) lenses, but since we are using scans we'll cancel the request; the program will then approximate. Then under the feature matching section of the ```Photos``` tab, we need to select ```Hugin's CPFind```, the method that will look for control points, or points that don't (or shouldn't) change bewteen images. It will then ask to choose which geometric algorithm should be used to match control points between the images, to optimize the alignment. We need to choose ```Position (y,p,r)``` to avoid further deformation.
The program will then match tridimensionally the images, that now need to be stitched to be able to have single images that can be overlapped.
To do this we need to go to the ```Stitcher``` tab, then ```Calculate optimal size```, then untick all the following boxes, except for ```No exposure correction, low dynamic range```, since we don't have actual photograps that need postproduction. Then we ```Stitch```.  
Now we can use enfuse to precisely overlap layers by opening the terminal and typing the following: ```{executable_path}/enfuse     --exposure-weight=0     --saturation-weight=0     --contrast-weight=1     --hard-mask     --output=output.tif INPUT*.tif```.


<div style="display: flex; flex-wrap: wrap;">
  <img src="https://github.com/giaaaacomo/APi-reg/assets/38686675/fbaa1118-cd39-48a3-b289-3ed22c109da8" width="250px" alt="text-1"><figcaption>Basic TIFF overlapping</figcaption>
  <img src="https://github.com/giaaaacomo/APi-reg/assets/38686675/1ccbc0b3-291d-4aaa-b38b-bbe039004d57" width="250px" alt="text-2"><figcaption>Enfuse</figcaption>
</div>


#### Align image stack
The process is the same of the above, except that we need to select ```Align image stack```, that should be precisely what we need. Unfortunately the control points optimization ends up giving alway ```error 1```, terminating the whole process. We were not able to overcome this problem.


# Conclusions  
The registration of different images containing a common picture seems to be still challenging through all the modern solutions available to the public. The computer vision algorithms used in FIJI date back up to the late 90's, but are still commonly used today in bioanalysis thanks to their specificity that allows for great precision in their native fields. Hugin could be able to provide a solution but at the actual point, the ```Align image stack``` is not working, and the other options provided don't actually match the images effectively.
This seems to be the perfect field for neural networks specialized in computer vision, but our research does not show signs of a specific tool made for this application as of now, available to the public.

# Future developments
    *peer-review of the project, both from scientist and from computer vision experts
    *better understanding of the fine-tuning, and new benchmark following this
    *wiki?
    *image elaboration through Google colab or similars  
    *implementation of CLIJ for GPU acceleration in FIJI
# Sources
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10819694/  
https://sites.google.com/site/qingzongtseng/template-matching-ij-plugin/tuto1
https://imagej.net/imaging/registration
