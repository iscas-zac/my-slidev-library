---
theme: seriph
background: /pics/cover.jpg
class: 'text-center'
highlighter: shiki
lineNumbers: false
info: |
  [non-euclidean paper at SIGGRAPH 2022](https://dl.acm.org/doi/pdf/10.1145/3528223.3530186)
drawings:
  enabled: true
  persist: false
css: unocss
---

# Modeling and Rendering Non-Euclidean Spaces approximated with Concatenated Polytopes

<div class="pt-12">
  SIGGRAPH 2022

  张弛 2022.11
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

<style>
  h1 {
    font-color: red
  }
</style>
---
layout: two-cols
---
# Introduction

... the general methods for rendering m-manifolds embedded in n-D Euclidean spaces.

![portal 2](/pics/portal2.jpg)
<div align="center">portal 2</div>




::right::

![antichamber](/pics/antichamber.jpg)
<div align="center">antichamber</div>
<!--
Some pictures like Portal 2 or else
-->

---

# Concepts
... mathematical backgrounds and methodologies ...

<v-click>

## manifold
A manifold is a topological space which looks locally like a Cartesian space, commonly a finite-dimensional Cartesian space $\mathbb{R}^n$.[^1]

</v-click>
<v-click>

## polytope
The notion of polytope is the generalization of the notion of polygon to arbitrary dimensions.[^2]

</v-click>
<div v-after>

[^1]: [nLab/manifold](https://ncatlab.org/nlab/show/manifold)
[^2]: [nLab/polytope](https://ncatlab.org/nlab/show/polytope)

</div>

<style>
.footnotes-sep {
  @apply mt-20 opacity-10;
}
.footnotes {
  @apply text-sm opacity-75;
}
.footnote-backref {
  display: none;
}
</style>

---

# Navigation and rendering in 2D-manifolds

a loop involving ray marching

<v-click>
<img src="/pics/camera_pos.png" width=250>

## camera.pose.update()

<font color=green>// get a normal vector using <Link to="5">cross operation</Link></font>
</v-click>
<v-click>

## step += delta
<font color=green>// the marching step size is sufficiently small</font>

</v-click>
<v-click>

## if ( $\mathbf{p}_0 + \alpha(\mathbf{p}_1 − \mathbf{p}_0) + \beta\mathbf{n} = \mathbf{s} + \gamma\mathbf{r}$ )

<font color=green>// If the ray of the ‘current’ marching step hits the infinite rectangle in the 3D Euclidean space, it is determined to hit the line segment in the 2-manifold.</font>
<style>
img {
  float: right;
  object-fit: fill;
}
</style>
<img src="/pics/manifold_path.png" width=500>

&emsp;&emsp;render()

### else
&emsp;&emsp;the ray is projected onto the manifold
</v-click>

---

# <i>cross</i> operation

 $$cross(\mathbf{v}_1, \mathbf{v}_2, \dots, \mathbf{v}_{n − 1}) := \bigstar(\mathbf{v}_1 \wedge \mathbf{v}_2 \wedge \dots \wedge \mathbf{v}_{n − 1})$$

## exterior product

$$ \wedge: \mathbb{R}^{n - 1} \times \dots \times \mathbb{R}^{n - 1} \to \mathbb{R}^{(n - 1) \times (n - 1)} $$

> #### Example
> in 3D space with orthonormal basis vectors, $(a\mathbf{e}_1 + b\mathbf{e}_2 + c\mathbf{e}_3) \wedge (d\mathbf{e}_1 +𝑒\mathbf{e}_2 + f\mathbf{e}_3) = (ae − bf)\mathbf{e}_1\mathbf{e}_2 + (bf − ce)\mathbf{e}_2\mathbf{e}_3 + (cd - af)\mathbf{e}_3\mathbf{e}_1$ (a bi-vector)

## Hodge star operator

$$ \bigstar: \mathbb{R}^{(n - m) \times (n - m)} \to \mathbb{R}^{(n - m) \times m} $$

> #### Example
> in 3D space with orthonormal basis vectors, $\bigstar ((ae − bf)\mathbf{e}_1\mathbf{e}_2 + (bf − ce)\mathbf{e}_2\mathbf{e}_3 + (cd - af)\mathbf{e}_3\mathbf{e}_1) = ((ae − bf)\mathbf{e}_3 + (bf − ce)\mathbf{e}_1 + (cd - af)\mathbf{e}_2)$

---

# Navigation in Polytopes

In order to reduce the computational cost, we approximate an 𝑚-manifold with a concatenation of 𝑚-D polytopes (henceforth, m-polytopes).


<crawler style="float: right; width:400px; height:360px" width="400" height="360"/>


<img id="right" src="/pics/polytope_path.png" width=400>

<img src="/pics/explain_polypath.png" width=300>


$\mathbf{r}' = \mathbf{r}'_0 + (\mathbf{I} − \mathbf{V}\mathbf{V}^T)\mathbf{r} = \mathbf{V}\mathbf{R}\mathbf{V}^T \mathbf{r} + (\mathbf{I} − \mathbf{V}\mathbf{V}^T)\mathbf{r}$


---

# Rendering in Polytopes

In order to reduce the computational cost, we approximate an 𝑚-manifold with a concatenation of 𝑚-D polytopes (henceforth, m-polytopes).

<img src="/pics/polytope_render.png" width=700 style="margin: 0 auto;">

<div align="center">multiple light-ray paths from a single light source to a single point</div>


---

# Scene modeling: polytope concatenation

minimize the lengths between mating facets (portals)

<video width=300 controls style="float: right; object-fit: fill;">
  <source src="/video/sfcr.mp4" type="video/mp4">
video not supported.
</video>

## <font color=LightGray>Spring forces and collision resolution</font>

<v-clicks>

**start: while (!timeout) { init_simulator(dimension); sf(); cr(); }**

**if(good) return;**
<font size="2" color=DarkGreen>// Alternating between spring forces and collision resolution is repeated until either the polytopes are concatenated with no interpenetration or the predefined count of iterations is reached.</font>

## <font color=LightGray>Adding joint polytopes</font>

**try { addjoint(); }**
<font size="2" color=DarkGreen>// The corresponding vertices of two mating facets are connectedafter exceeding the predefined iteration count.</font>

## <font color=LightGray>Embedding in higher-dimensional spaces</font>
**catch(e) { dimension++; goto start; }**
</v-clicks>

---

# Effects

<style>
  .container {
    -ms-overflow-style: auto;  /* Internet Explorer 10+ */
    scrollbar-width: auto;  /* Firefox */
}
.container::-webkit-scrollbar { 
    display: auto;  /* Safari and Chrome */
}
  div.scroll {
    overflow-x: scroll;
    overflow-y: hidden;
    white-space: nowrap;
    margin: 0 auto;
  }
</style>

<div class="scroll" style="margin: auto;">
  <div style="display: inline-block; width: 100%;">
    <video width=650 controls style="margin: 0 auto;">
      <source src="/video/rt_result.mp4" type="video/mp4">
    video not supported.
    </video>
    <div align="center"> ray tracing effect </div>
  </div>
  <div style="display: inline-block; width: 100%;">
    <video width=650 controls style="margin: 0 auto;">
      <source src="/video/phong_result.mp4" type="video/mp4">
    video not supported.
    </video>
    <div align="center"> phong lighting effect </div>
  </div>
  <div style="display: inline-block; width: 100%;">
    <video width=650 controls style="margin: 0 auto;">
      <source src="/video/pt_result.mp4" type="video/mp4">
    video not supported.
    </video>
    <div align="center"> path tracing effect </div>
  </div>
</div>

---

# <div style="background-color=red;">NEXT TIME ...</div>

<v-click>

## Generalized Resampled Importance Sampling: Foundations of ReSTIR

<video width=650 controls style="margin: 0 auto;">
  <source src="/video/next.mp4" type="video/mp4">
video not supported.
</video>

</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>