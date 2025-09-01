===============
Computer Vision
===============

1. `OpenCV`_

`back to top <#computer-vision>`_

OpenCV
======

* `Image Read/Write`_, `Mat Class`_
* Open Source Computer Vision: free computer vision library to manipulate images and videos


Image
-----
    * supported image formats: bmp, pbm, pgm, ppm, sr, ras, jpeg, jpg, jpe, jp2, tiff, tif,
      png, OpenEXR
    * **Image Representation**
        - represented as multi-dimensional array, 2D or 3D of ``numpy.array``
        - 1st index: y coordinate or row
        - 2nd index: x coordinate or column
        - 3rd index: color channel
    * **Image Manipulation**
        - NumPy array operations are useful for image manipulations in OpenCV
        - use OpenCV's or NumPy's array slicing to manipulate a large region of an image
        - define ROI (regions of interests), and perform operations on it

        .. code-block:: python

           img = cv2.imread("Pic.png")
           img[100, 120, 0] = 123  # set single pixel's blue channel
           img[:, :, 1] = 0  # set all G values to 0
   
           # copy a portion over to another position
           my_roi = img[0:100, 0:100]
           img[300:400, 300:400] = my_roi


    * ``imread(src, mode)``
        - load image using the file path, specifying ``mode`` is optional
        - return an image in BGR colour format by default
        - example modes: ``IMREAD_COLOR``, ``IMREAD_UNCHANGED``, ``IMREAD_GRAYSCALE``
        - data read is stored in ``Mat`` object
    * ``imwrite(path, src)``
        - write to a file
        - requires ``src`` to be in BGR or grayscale format that the output format can support
    * ``samples.findFile()``: images from OpenCV samples
    * ``imshow(title, Mat_obj)``
        - show image
        - ``waitKey(time)``: wait for user input, and return the value of the key pressed
    * ``cvtColor(src, code, dst)``
        - convert image to another colour space

Mat Class
---------
    * **Matrix Header**
        - contain information such as the size of the matrix, storing method, and matrix
          address
        - constant size
        - can have headers which refer to only a subsection of the full data
    * **Pointer**
        - points to the matrix with the pixel values
        - matrix size varies from image to image
    * each Mat object has own header, but a matrix may be shared by having same pointer values
    * assignment and copy operators only copy the headers and the pointer
    * the last object that used the Mat object cleans it up
    * ``Mat_OBJ.clone()``
        - clone the matrix
    * ``Mat_OBJ.copyTo(Mat_OBJ)``
        - copy the matrix to another object

`back to top <#computer-vision>`_
