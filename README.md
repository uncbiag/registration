# Image registration

The purpose of this repository is to provide an overview of github repositories on non-parametric image registration.

All our code is written in python using [PyTorch](https://pytorch.org/) except for our original deep-learning registration work [[Yang16]](#yang16) which is written in lua using [torch](http://torch.ch/). 

<hr>


# Mermaid: iMagE Registration via autoMAtIc Differentiation

<pre>
                                      _     _ 
                                     (_)   | |
  _ __ ___   ___ _ __ _ __ ___   __ _ _  __| |
 | '_ ` _ \ / _ \ '__| '_ ` _ \ / _` | |/ _` |
 | | | | | |  __/ |  | | | | | | (_| | | (_| |
 |_| |_| |_|\___|_|  |_| |_| |_|\__,_|_|\__,_|
                                                                                      
 </pre>                                       

Mermaid is a registration toolkit making use of automatic differentiation for rapid prototyping.
It includes various image registration models. In particular, stationary velocity field models
 (both based on velocity fields and momentum fields), scalar vector momentum Large
  Displacement Diffeomorphic Metric Mapping (LDDMM) models as well as the more generalized Region-specific Diffeomorphic Metric Mapping model (RDMM).


The Mermaid repository can be found here:
https://github.com/uncbiag/mermaid

**Supported transformation models**:
* affine_map: map-based affine registration
* diffusion_map: displacement-based diffusion registration
* curvature_map: displacement-based curvature registration
* total_variation_map: displacement-based total variation registration
* svf_map: map-based stationary velocity field
* svf_image: image-based stationary velocity field
* svf_scalar_momentum_image: image-based stationary velocity field using the scalar momentum
* svf_scalar_momentum_map: map-based stationary velocity field using the scalar momentum
* svf_vector_momentum_image: image-based stationary velocity field using the vector momentum
* svf_vector_momentum_map: map-based stationary velocity field using the vector momentum
* lddmm_shooting_map: map-based shooting-based LDDMM using the vector momentum
* lddmm_shooting_image: image-based shooting-based LDDMM using the vector momentum
* lddmm_shooting_scalar_momentum_map: map-based shooting-based LDDMM using the scalar momentum
* lddmm_shooting_scalar_momentum_image: image-based shooting-based LDDMM using the scalar momentum
* lddmm_adapt_smoother_map: map-based shooting-based Region specific diffemorphic mapping, with a spatio-temporal regularizer
* svf_adapt_smoother_map: map-based shooting-based vSVF, with a spatio regularizer

**Supported similarity measures**:
* ssd: sum of squared differences
* ncc: normalize cross correlation
* ncc_positive: positive normalized cross-correlation
* ncc_negative: negative normalized cross-correlation
* lncc: localized normalized cross correlation (multi-scale)

**Supported solvers**:
* embedded RK4
* torchdiffeq: explicit_adams, fixed_adams, tsit5, dopri5, euler, midpoint, rk4

**Optimizer**:
* support single/multi-scale optimizer
* support SGD, l-BFGS and some limited support for adam

<hr>


# EasyReg

EasyReg is an extension that builds on Mermaid, providing a simple interface to Mermaid and other popluar registration packages.

The currently supported methods include Mermaid-optimization (i.e., optimization-based registration) and Mermaid-network (i.e., deep network-based registration methods using the mermaid deformation models, like SVF, LDDMM, RDMM...).
We also added some supports on [ANTsPy](https://github.com/ANTsX/ANTsPy), [NiftyReg](http://cmictig.cs.ucl.ac.uk/wiki/index.php/NiftyReg) and Demons(embedded in [SimpleITK](http://www.simpleitk.org/SimpleITK/resources/software.html)).

The EasyReg repository can be found here:
https://github.com/uncbiag/easyreg

<hr>


# Gallery


Here are some examples:

**Vector Momentum-parameterized Stationary Velocity Field(vSVF)**: with a temporal-invariant velocity field and a constant regularizer.

<img src="images/vsvf.gif" alt="vsvf" width="700"/><br>


**Large Displacement Diffeo-morphic Metric Mapping (LDDMM)**: with a spatial-temporal velocity field and a constant regularizer.

<img src="images/lddmm.gif" alt="lddmm" width="700"/><br>


**Region-specific Diffeomorphic Metric Mapping (RDMM)**: with a spatial-temporal velocity field and a spatial-temporal regularizer.

RDMM with an optimized regularizer

<img src="images/rdmm.gif" alt="rdmm" width="700"/><br>

RDMM with a pre-defined regularizer

<img src="images/rdmm_predefined.gif" alt="rdmm_predefined" width="700"/><br>

<hr>


# Related work

The first work on deep-learning for non-parametric medical image registration [[Yang16]](#yang16) predicted the initial momentum of an LDDMM solution, hence was already able to guarantee diffeomorphic transformations. Because it is based on patch-wise predictions, it can be applied to very large images if needed. This work was extended and extensively analyzed in [[Yang17]](#yang17), showing competitive performance with state-of-the-art optimization-based deformable image registration approaches; it also included deep-network models to predict displacement and velocity fields. Follow-up work focused on learning the registration metric [[Niethammer19]](#niethammer19); jointly learning to predict affine transformations and a nonparametric transformation [[Shen19a]](#shen19a) (including multi-step approaches and training to encourage symmetric models); as well as extensions of the LDDMM approach to spatio-temporal regularizers (i.e., that move with the deforming images) also within a deep-network approach [[Shen19b]](#shen19b). 

Bibtex entries for this related work can be found here [[bibtex]](citations.bib) and the citations are listed in the next section.

# Bibliography

<a id="yang16">[Yang16]</a> Fast Predictive Image Registration [[pdf]](https://arxiv.org/pdf/1607.02504.pdf) [[code]](https://github.com/rkwitt/FastPredictiveImageRegistration)\
Xiao Yang, Roland Kwitt, Marc Niethammer. DLMIA 2016.

<img src="images/dlmia_network.png" alt="fast_predictive" width="300"/><br>


<a id="yang17">[Yang17]</a> Quicksilver: Fast predictive image registration--a deep learning approach [[pdf]](https://arxiv.org/pdf/1703.10908.pdf) [[code]](https://github.com/rkwitt/quicksilver)\
Xiao Yang, Roland Kwitt, Martin Styner, Marc Niethammer, NeuroImage 2017.

<img src="images/neuroimage_quicksilver.png" alt="neuroimage_quicksilver" width="300"/><br>


<a id="niethammer19">[Niethammer19]</a> Metric Learning for Image Registration [[pdf]](https://arxiv.org/pdf/1904.09524.pdf) [[code]](https://github.com/uncbiag/mermaid)\
Marc Niethammer, Roland Kwitt, Francois-Xavier Vialard. CVPR 2019.

<img src="images/metric_learning.png" alt="metric_learning" width="300"/><br>


<a id="shen19a">[Shen19a]</a> Networks for Joint Affine and Non-parametric Image Registration [[pdf]](https://arxiv.org/pdf/1903.08811.pdf) [[code]](https://github.com/uncbiag/easyreg)\
Zhengyang Shen, Xu Han, Zhenlin Xu, Marc Niethammer. CVPR 2019.

<img src="images/avsm.png" alt="avsm" width="300"/><br>


<a id="shen19b">[Shen19b]</a> Region-specific Diffeomorphic Metric Mapping [[pdf]](https://arxiv.org/pdf/1906.00139.pdf) [[code]](https://github.com/uncbiag/easyreg)\
Zhengyang Shen, François-Xavier Vialard, Marc Niethammer. NeurIPS 2019.

<img src="images/rdmm.png" alt="rdmm" width=400><br>
<hr>
