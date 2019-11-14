This article is face to readers who are not familiar with
**fluid-based** image registration, and the object is to let them be
interested in this topic :)  
I will try to make the whole story non-painful. As an overview, let’s
see what we are going to talk about.

-   what is the fluid-based image registration

-   why fluid-based models

-   Some typical models

Hmmm, let’s start....
$$
\frac{n!}{k!(n-k)!} = {n \choose k}
$$
What is the fluid-based image registration
==========================================
![\sum_{i=0}^{N}\LaTeX_{i}](https://latex.codecogs.com/gif.latex?\sum_{i=0}^{N}\LaTeX_{i})
![\sum_{i=0}^{N}\LaTeX_{i}](https://latex.codecogs.com/gif.latex?{v^* = \underset{v}{\text{argmin}}~ \frac 12 \int_0^1 \|v(x,t) \|^2_L \mathrm{d} t + \operatorname{Sim}(I(1),I_1)})
Before we start to answer what is fluid-based image registration?

Let’s first review what image registration is: *Image registration is to
find a spatial transformation between image pairs, the source image (or
moving image) and the target image (or fixed image).*

Smile.gif

A desired transformation can be evaluated by “similarity” and
“smoothness” that we often refer to “regularization”.

There were bunches of registration methods, like rigid, B-spline,
elastic to name a few. For those who are interested in those
professional topics, please refers to “numerical methods for image
registration” (enjoy your reading :))

Back to fluid based image registration, literally, we view the image
registration process as a continuous flow; both images are taken as
fluids, and the source image is flow toward to target image Recall that
there is a continuous time axis, so the flow is dynamic).

Before step further, let’s would introduce some notations to help
formulate the fluid registration problem. Let’s denote source image as
*I*<sub>0</sub>, target image as*I*<sub>1</sub>. The image coordinate as
$\\bf x$ (typically a 2d or 3d vector). To simplify notation, we will
not bold $\\bf x$ in the rest of the article.

In fluid-based image registration method, each location typically
discreted by image coordinate x has its own velocity at time point t,
v(x,t), it can either be temporally variant *v*(*x*, *t*) or -invariant
*v*(*x*, *t*) = *v*(*x*, 0) during the registration. For the image at
time point t (here we assume *t* ∈ \[0, 1\]), we denote it as *I*(*t*).
*I*(1) refers to the resulting warped image (compared with
*I*<sub>1</sub>, the target image).

Back to the “similarity” and “smoothness”. On one hand, we are try to
get an *I*(1) that close to *I*<sub>1</sub>. The term "close" depends on
the similarity metric. One the other hand, we would like to regularize
the flow to be an continuous, both spatially and temporally, flow. So we
can define the problem like this
$$v^\* = \\underset{v}{\\text{argmin}}~ \\frac 12 \\int\_0^1 \\|v(x,t) \\|^2\_L \\mathrm{d} t + \\operatorname{Sim}(I(1),I\_1),
\\label{eq:lddmm}$$

The *L* here refers the regularizer that encourages the property of the
velocity field. So the first term is the regularization term while the
second term is the similarity term. The objective is to find the optimal
velocity filed *v*<sup>\*</sup>(*x*, *t*).

So far so good? Now, let’s introduce the famous advection equation. We
simply the notation by denoting *I* = *I*(*x*, *t*) and
*v* = *v*(*x*, *t*). Besides, ⟨⋅, ⋅ ⟩ refers to dot product and ∇ refers
to spatial gradient.
∂<sub>*t*</sub>*I* + ⟨∇*I*, *v*⟩ = 0
It may tell you nothing at the first glance (at least, I felt like
that). To those who interests where it froms, here is the link
(<https://www.youtube.com/watch?v=atvw5iseoGQ>). A short summary is
according to this equation, once we have *I*(*x*, *t*) and v(x,t), we
can moving the image *I*(*x*, *t*) according to *v*(*x*, *t*) ( the v
here is extactly the same as what you understand in physical way) and
get the next time step image by computing
*I*(*t* + *δ**t*) = *I*(*t*) + ⟨∇*I*, *v*⟩*δ**t*.

Finally, For the typical fluid-based registration, we get the
formulation like this
$v^* = \underset{v}{\text{argmin}}~ \frac 12 \int_0^1 \|v(x,t) \|^2_L \mathrm{d} t + {Sim}(I(1),I_1),$

Typical models
==============

Now, let’s start explore a family of fluid-based registration. Here is
the full menu.

-   Vector Momentum-parameterized Stationary Velocity Field (vSVF)

-   Large Displacement Diffeo-morphic Metric Mapping (LDDMM)

-   Region-specific Diffeomorphic Metric Mapping (RDMM)

One more time, let’s recall *v*(*x*, *t*) determines the how image
*I*(0) flows into *I*(1) and the regularizer *L*(*x*, *t*) determines
how *v*(*x*, *t*) is regularized.

The regularizer *L* is can be anything, like LASSO, Least Square ...
Here we may skip some awsome math**<sup>\*</sup>
([link](http://campar.in.tum.de/twiki/pub/DefRegTutorial/WebHome/MICCAI_2010_Tutorial_Def_Reg_Darko.pdf)).
We introduce momentum m, and velocity field can be smoothed from
*v* = *G* \* *m*, where *G* is gaussian smoother.

*\*(Optional) The key idea is to get a velocity field v, which can be
define in Sobolev spaces, where
*H*<sup>*s*</sup> ≡ {*v*:∥*v*∥<sub>*H*<sup>*s*</sup></sub>&lt;∞} with
$\\langle u, h\\rangle\_{\\mathrm{H}^{s}}=\\sum\_{k=0}^{s}\\left\\langle\\mathrm{D}^{k} u, \\mathrm{D}^{k} h\\right\\rangle\_{\\mathrm{L}^{2}}$,
meaning sum of any order derivatives on *v* are bounded.
∥*v*∥<sub>*L*</sub><sup>2</sup> = ⟨*L*<sup>†</sup>*L**v*, *v*⟩, we
introduce *m* = *L*<sup>†</sup>*L**v* or
*v* = (*L*<sup>†</sup>*L*)<sup> − 1</sup>*m*. According Green’s
function, assume *v* is in Sobolev spaces and *s* = ∞, it is equivalent
to get *v* = (*L*<sup>†</sup>*L*)<sup> − 1</sup>*m* = *G* \* *m**

The advantage of introducing momentum *m* is that we can optimize over
momentum *m*, even if the optimal m is not smooth, we can get an smooth
*v* by applying gaussian smoother. In practice, the multi-gaussian
smoother allows multi-scale regularier and can get better registration
results.

mtov.png

Before, we get into each method, let’s first see how these methods
related.

tree.png

Vector Momentum-parameterized Stationary Velocity Field (vSVF)
--------------------------------------------------------------

The relative simple model is Momentum-parameterized Stationary Velocity
Field (vSVF), where it assume the velocity field *v* is temporal
invariant, which refers the momentum *m* is also temporal invariant.
Besides the regularizer(or you can just view it as a gaussian smoother)
is fixed or spatio-temporal invariant.

See how vSVF performs:

The following part is optional:

We have *v*(*x*, *t*) = *v*(*x*, 0), and
*L*(*x*, *t*) = *c**o**n**s**t*, so
∫<sub>0</sub><sup>1</sup>∥*v*(*x*, *t*)∥<sub>*L*</sub><sup>2</sup>*d**t* = |*v*(*x*, 0)∥<sub>*L*</sub><sup>2</sup> = ⟨*m*(*x*, 0), *v*(*x*, 0)⟩
the Eq.
<a href="#eq:general" data-reference-type="ref" data-reference="eq:general">[eq:general]</a>
turns into
$$\\begin{split}
&m(x,0)^\* = \\underset{v}{\\text{argmin}}~ \\frac 12 \\langle m(x,0),v(x,0)\\rangle  + \\operatorname{Sim}(I(1),I\_1),\\\\ &\\quad\\text{s.t.}\\quad\\partial\_t I + \\langle \\nabla I, v(x,0)\\rangle=0;\\quad I(0)=I\_0.
\\end{split}$$

The objective is to search optimal *m*(*x*, 0), which determines how the
image flows.

Large Displacement Diffeo-morphic Metric Mapping (LDDMM)
--------------------------------------------------------

Maybe you have noticed the limits of the vSVF, since the v(x,t) is fixed
along time axes, which can be a large constrain. So let’s remove it, and
then comes famous Large Displacement Diffeo-morphic Metric Mapping
(LDDMM). In LDDMM, the v(x,t)= v(x,t)! However, the regularizier (or you
can just view it as a gaussian smoother), like vSVF, is fixed.

See how LDDMM performs:

The following part is optional:

We have *v*(*x*, *t*), and *L*(*x*, *t*) = *c**o**n**s**t*, but it can
be proven, in optimal condition (beyond this introduction), the energy
is conserved,
∫<sub>0</sub><sup>1</sup>∥*v*(*x*, *t*)∥<sub>*L*</sub><sup>2</sup>*d**t* = |*v*(*x*, 0)∥<sub>*L*</sub><sup>2</sup> = ⟨*m*(*x*, 0), *v*(*x*, 0)⟩.
The Eq.
<a href="#eq:general" data-reference-type="ref" data-reference="eq:general">[eq:general]</a>
turns into
$$\\begin{split}
&m(x,0)^\* = \\underset{v}{\\text{argmin}}~ \\frac 12 \\langle m(x,0),v(x,0)\\rangle  + \\operatorname{Sim}(I(1),I\_1),\\\\ &\\quad\\text{s.t.}\\quad\\partial\_t I + \\langle \\nabla I, v(x,t)\\rangle=0;\\quad I(0)=I\_0.
\\end{split}$$
The objective is to search optimal *m*(*x*, 0), which determines how
mometum *m*(*x*, *t*) evolves, since the velocity is smoothed from *m*,
it indirectly determines how the image flows.

Region-specific Diffeomorphic Metric Mapping (RDMM)
---------------------------------------------------

At this point, you may already predict the improvement in RDMM (if you
ignore the family tree above.):) *L*(*x*, *t*) = *L*(*x*, *t*)! Every
parameter own its freedom. And yes, RDMM is a generalization on the
LDDMM. In RDMM, we give the *L* full freedom. But we add a reasonable
assumption, the *L*(*x*, *t*) flows with the image,*I*(*t*), So what’s
the influence? Region-specific regularizer, which means each region
(pixel level) has its own regularizer during the registration. The
regularizer tracks how image moves. If at *t* = 0, the regularizer of a
region is large, then this region will not experience crazy deformation,
vice versa. So let’s see how RDMM with a pre-defined regularizer
peforms.

But since RDMM is a generalization of LDDMM, can it perform regular
task? Of course, see the following task with an optimized regularizer.

The following part is optional:

First, how the regularizer *L*(*x*, *t*) tracks the image *I*(*t*)? The
advevtion equation. The same idea on advect the image.
 ∂<sub>*t*</sub>*L* + ⟨∇*L*, *v*⟩ = 0<sup>\*</sup>

*\* The regularizer *L* is a multi-gaussian smoother, so in practice,
the parameters of the smoother is advected. To ensure the spatial
smoothness of the mutli-gaussian smoother(since each location has its
own smoother with different parameters), a pre-parameter (similar idea
like introducing momentum) is introduced. All these are beyond this
introduction, for those who are interests, please refer to
to[RDMM](https://arxiv.org/pdf/1906.00139.pdf)*

It can be proven, in optimal condition (beyond this introduction) of a
specific formulation between *v* and *L* can conserve energy of term
∫<sub>0</sub><sup>1</sup>∥*v*(*x*, *t*)∥<sub>*L*(*x*, *t*)</sub><sup>2</sup>*d**t*,
∫<sub>0</sub><sup>1</sup>∥*v*(*x*, *t*)∥<sub>*L*(*x*, *t*)</sub><sup>2</sup>*d**t* = |*v*(*x*, 0)∥<sub>*L*(*x*, 0)</sub><sup>2</sup> = ⟨*m*(*x*, 0), *v*(*x*, 0)⟩.
The Eq.
<a href="#eq:general" data-reference-type="ref" data-reference="eq:general">[eq:general]</a>
turns into
$$\\begin{split}
&m(0)^{\*},L(x,0)^{\*}=\\underset{m(0),L(x,0)}{\\operatorname{argmin}} \\frac{1}{2}\\langle m(x,0),v(x,0)\\rangle+\\operatorname{sim}\\left(I(1), I\_{1}\\right)+\\operatorname{Reg}\\left(L(x,0)\\right)\\\\
\\quad\\text{s.t.}&\\quad\\partial\_t I + \\langle \\nabla I, v)\\rangle=0, \\quad\\partial\_t L + \\langle \\nabla L,v\\rangle=0, \\quad I(0)=I\_0.
\\end{split}$$
The objective is to search optimal *m*(*x*, 0) and *L*(*x*, 0), which
determines how mometum *m*(*x*, *t*) and the regularizer *L*(*x*, *t*)
evolve, since the velocity is smoothed from *m* by *L*(*x*, *t*), it
indirectly determines how the image flows.