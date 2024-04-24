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
* Image files of the same format and size (although not necessary for all the solutions, it's a good way to be sure to be able to experiment with different tools)

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
All of this leads to a steep selection in the available plugins (that will be explained further in the next chapter) and we ended up trying the following:
1. [Elastic Stack Alignment](https://imagej.net/plugins/elastic-alignment-and-montage)
2. [Descriptor-based series registration]
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
    
  **review**
  > **pros:** tons of options, super precise, super versatile, super customizable  
  > **cons:** tons of options, hardware intensive (especially CPU and RAM), few plugins support GPU acceleration, (therefore) slower


### * hugin

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
    1. import all the images as a serie (file > import > import serie of images)
    2. go to plugin, head to registration, select the desired plugin
#### FIJI PLUGINS
  **disclaimer**
  >If you are using a computer with many CPU cores, you need to make sure that you have enough memory (RAM) to handle your images for alignment. In general, you'll need 10x the file size of your largest 2D image for each core (see: https://imagej.net/plugins/trakem2/#how-much-ram-should-i-allocate-to-the-jvm-for-fiji-to-run-trakem2). EXAMPLE: If your dataset consists of images that are 600MB each and your computer has 12 cores, you will need at least 72 GB of RAM (= 600 MB x 10 x 12 cores).  
cit. The University of Texas at Austin (Texas Advanced Computing Center)
 
 >in our tests we used 32 images about ~3mb each, that ended up using up to 96% of the 16gbs of ram. Often, ram saturation ends up prematurely the process giving an error. The disclaimer above was written for the elastic alignment but should be followed as a rule of thumb for all the plugins. 
* elastic alignment
### Hugin

# Future developments
    *image elaboration through Google colab or similars  
    *implementation of CLIJ for GPU acceleration in FIJI
# Sources
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10819694/  
https://sites.google.com/site/qingzongtseng/template-matching-ij-plugin/tuto1
