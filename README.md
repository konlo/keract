# Keract: Keras Activations + Gradients

[![Downloads](https://pepy.tech/badge/keract)](https://pepy.tech/project/keract)
[![Downloads](https://pepy.tech/badge/keract/month)](https://pepy.tech/project/keract)
```bash
pip install keract
```
*You have just found a (easy) way to get the activations (outputs) and gradients for each layer of your Keras model (LSTM, conv nets...).*

<p align="center">
  <img src="assets/intro.png">
</p>

## API

- [get_activations](#get-activations-outputs-of-each-layer)
- [display_activations](#display-the-activations-youve-obtained)
- [display_heatmaps](#display-the-activations-as-a-heatmap-overlaid-on-an-image)
- [get_gradients_of_trainable_weights](#get-gradients-of-weights)
- [get_gradients_of_activations](#get-gradients-of-activations)

### Get activations (outputs of each layer)

```python
from keract import get_activations
activations = get_activations(model, x, layer_name)
```

Inputs are:
- `model` is a `keras.models.Model` object.
- `x` is a numpy array to feed to the model as input. In the case of multi-input, `x` is of type List. We use the Keras convention (as used in predict, fit...).
- `layer_name` (optional) - the layer to get activations for if you only want the activations for one layer

The output is a dictionary containing the activations for each layer of `model` for the input x:

```
{
  'conv2d_1/Relu:0': np.array(...),
  'conv2d_2/Relu:0': np.array(...),
  ...,
  'dense_2/Softmax:0': np.array(...)
}
```

The key is the name of the layer and the value is the corresponding output of the layer for the given input `x`.

### Display the activations you've obtained

```python
from keract import display_activations
display_activations(activations, cmap="gray", save=False)
```

Inputs are:
- `activations` a dictionary mapping layers to their activations (the output of get_activations)
- `cmap` (optional) a string of a valid matplotlib colourmap
- `save`(optional) a bool, if True the images of the activations are saved rather than being shown

### Display the activations as a heatmap overlaid on an image

```python
from keract import display_heatmaps
display_heatmaps(activations, input_image, save=False)
```

Inputs are:
- `activations` a dictionary mapping layers to their activations (the output of get_activations)
- `input_image`  a numpy array of the image you inputed to the get_activations
- `save`(optional) a bool, if True the images of the activations are saved rather than being shown
### Get gradients of weights
- `model` is a `keras.models.Model` object.
- `x` Input data (numpy array). Keras convention.
- `y`: Labels (numpy array). Keras convention.

```python
from keract import get_gradients_of_trainable_weights
get_gradients_of_trainable_weights(model, x, y)
```

The output is a dictionary mapping each trainable weight to the values of its gradients (regarding x and y).

### Get gradients of activations

- `model` is a `keras.models.Model` object.
- `x` Input data (numpy array). Keras convention.
- `y`: Labels (numpy array). Keras convention.

```python
from keract import get_gradients_of_activations
get_gradients_of_activations(model, x, y)
```

The output is a dictionary mapping each layer to the values of its gradients (regarding x and y).

## Examples

Examples are provided for:
- `keras.models.Sequential` - mnist.py
- `keras.models.Model` - multi_inputs.py
- Recurrent networks - recurrent.py

In the case of MNIST with LeNet, we are able to fetch the activations for a batch of size 128:

```
conv2d_1/Relu:0
(128, 26, 26, 32)

conv2d_2/Relu:0
(128, 24, 24, 64)

max_pooling2d_1/MaxPool:0
(128, 12, 12, 64)

dropout_1/cond/Merge:0
(128, 12, 12, 64)

flatten_1/Reshape:0
(128, 9216)

dense_1/Relu:0
(128, 128)

dropout_2/cond/Merge:0
(128, 128)

dense_2/Softmax:0
(128, 10)
```

We can visualise the activations. Here's another example using VGG16:

```
cd examples
pip install -r examples-requirements.txt
python vgg16.py
```

<p align="center">
  <img src="assets/cat.jpg">
  <br><i>A cat.</i>
</p>


<p align="center">
  <img src="assets/cat_activations.png" width="600">
  <br><i>Outputs of the first convolutional layer of VGG16.</i>
</p>

Also, we can visualise the heatmaps of the activations:

```
cd examples
pip install -r examples-requirements.txt
python heat_map.py
```

<p align="center">
  <img src="assets/heatmap.png">
</p>
