# stylegan_style_transfer
 
# General information

We used a pre-trained stylegan(Nvidia) on the WikiArt dataset.
We tried to manipulate the style of some images, using three different methods.
In each method we used the same content and style images as inputs, to see the
different impacts on the target images. We tested two different content and
style image for those methods:


**Style Mixing**
In this method, we tried to achieve the style transfer using the style mixing
mechanism. For each test, we used the w_content and the w_style, and used
different levels from each of the w’s as the input to the stylegan synthesis
network, to see how different levels affected the output. Let's see the results of
using different levels:

Content image 1 and Style image 1

```
content_w levels: 5 - 18, style_w levels: 1- 4
```


   - content_w levels: 8 - 18, 1- 4, style_w levels: 5-
- content_w levels:1 - 7, 9 - 18, style_w levels: 8-
- content_w levels:1 - 11, 16 - 18, style_w levels: 12-


As shown above, when we used the lower levels of w with the levels from
style_w, we saw that the output image has taken the shape and structure of the
style image, and from the content image mainly the colors and the little details,
like the face of the woman. When we used the last levels from style_w, we saw
that now the shape and structure are mainly formed from the content image, and
the colors from the style image.

**Optimization in W+**
In this method, we tried to optimize W+. We initially allocated W+ as a repetition
of 18 w’s of the content image. For the optimization, we used Adam optimizer
with a learning rate of 0.01, with 500 iterations. The loss function uses content
loss and Gram-based style loss, with weights of 0.4 to the content loss and
0.6 for the style loss. If we give the style loss more weight, the output image is affected more by the style image. For the content loss, we extracted the
features from the content image and the target image, using a specific layer from
the VGG network, and compared the content features and the target features
using Mse loss. For the style loss, we extracted the features from the style image
and the target image, using all of the layers from the VGG network, and then
compared the gram matrices of the style features maps and the gram matrices of
the target features maps using Mse loss. Let's see the gradual(left to right) and
final results of the optimization:


Image 1 loss Image 2 loss


**Optimization in S**
In this method, we tried to optimize s. In order to get s space, which is achieved
by an affine transformation on w, in each layer block in the stylegan synthesis
network, we extract the affine functions from the network, use them on w_content
and keep the outputs as the initial s. After that, we override the forward functions
of the synthesis network components, with forward functions that do not use the
affine transformation, instead, they get as directly as the input. For the
optimization, we used Adam optimizer with a learning rate of 0.01, with 500
iterations. The loss function uses content loss and Gram-based style loss, with
weights of 0.4 for the content loss and 10 for the style loss, to give more
attention to the style. For the content loss, we extracted the features from the
content image and the target image, using a specific layer from the VGG
network, and compared the content features and the target features using Mse
loss. For the style loss, we extracted the features from the style image and the
target image, using all of the layers from the VGG network, and then compared
the gram matrices of the style features maps and the gram matrices of the target
features maps using Mse loss. Let's see the gradual(left to right) and final results
of the optimization:


Image 1 loss Image 2 loss


**Conclusions**

Our goal was to modify the style of the content image to match
the style image and to keep the large-scale content of the content image. In the
style mixing method, the best results were achieved using the 11-15 levels of the
w_style, on the other levels we can see the interpolation of the content and style
images, but failed to keep the main features of the content image. On the W+
and S optimization, the output images have achieved our goal, the output images
keep a large scale of the content image features, and combine the style features
in an elegant way. W+ and S methods worked better because of their mechanism
of improving both the content loss and style loss, while style mixing it’s not
the main goal. The main improvement on W+ and S has gained during the first
100 iterations, the rest of the iterations improve the loss, but not a significant
change on the output image. S optimization has achieved better results than the
rest of the methods, we assumed it related to the large number of parameters
that are being learned in S optimization.



