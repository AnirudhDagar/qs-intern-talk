+++
title = "Anirudh's Internship Presentation"
outputs = ["Reveal"]
+++

# Array Libraries Interoperability
Anirudh Dagar


{{< social >}}
{{< talk-link qs-intern-talk >}}


{{< figure src="images/labs.jpg" height="60px" >}}


---

{{% grid middle %}}


{{< g 1 >}}

# Array Providers

{{% fragment %}} PyTorch {{% /fragment %}}
{{% fragment %}} NumPy {{% /fragment %}}
{{% fragment %}} CuPy {{% /fragment %}}
{{% fragment %}} MXNet {{% /fragment %}}
{{% fragment %}} JAX etc. {{% /fragment %}}

{{< /g >}}


{{< g 1 >}}

# Array Consumers

{{% fragment %}} SciPy {{% /fragment %}}
{{% fragment %}} scikit-learn {{% /fragment %}}
{{% fragment %}} scikit-image {{% /fragment %}}
{{% fragment %}} AstroPy {{% /fragment %}}
{{% fragment %}} einops etc. {{% /fragment %}}

{{< /g >}}


{{% /grid %}}

---


> NumPy, CuPy, PyTorch or SciPy aren't gonna hit as hard as all of them when used together.
> But it ain't about finding a way to use them individually; it's about making them work together.
> That's how interoperable science is done.
> <cite>Rocky Balboa</cite>

{{% note %}}
Hopefully you've seen this very popular boxing movie series "Rocky".
{{% /note %}}

---

## Try using non-NumPy with SciPy

```python{1-3|6|6-12}
# Estimate power spectral density using Welch’s method.
# SciPy offers a method `welch` for doing exactly that.
from scipy.signal import welch


freq, Pxx = welch(x)

# For CuPy
ValueError: object __array__ method not producing an array

# For PyTorch
TypeError: can't convert cuda:0 device type tensor to numpy.
```


---

# Python Array API

- {{< frag c="Standard Array API across all array/tensor frameworks." >}}
- {{< frag c="NEP 47 or __array_namespace__ protocol" >}}
- {{< frag c="https://data-apis.org/array-api/latest/" >}}


{{% note %}}
The APIs of each of these libraries are largely similar, but with enough differences that it’s quite difficult to write code that works with multiple (or all) of these libraries. This array API standard aims to address that issue, by specifying an API for the most common ways arrays are constructed and used.
{{% /note %}}

---

# Python Array API: Use Case

- {{< frag c="add hardware accelerator and distributed support to SciPy" >}}
- {{< frag c="simplify einops by removing the backend system" >}}

---

# Array API Demo: SciPy with PyTorch Tensors

---

# PyTorch Contributions

Improve Array API Compliance in PyTorch

<ul>
  <li class="fragment"><a href="https://github.com/pytorch/pytorch/pull/62560">torch.concat (PR #62560)</a></li>
  <li class="fragment"><a href="https://github.com/pytorch/pytorch/pull/63285">torch.linalg.cross (PR #63285)</a></li>
  <li class="fragment"><a href="https://github.com/pytorch/pytorch/pull/63227">torch.linalg.matmul (PR #63227)</a></li>
</ul>

---

# ``uarray``

- {{< frag c="backend / dispatch mechanism" >}}
- {{< frag c="separately define an API; backends contain separate implementations of that API." >}}
- {{< frag c="user can register an implementation" >}}

---

# uarray Tracker : Issue #14353

- [x] `scipy.fft` [PR #10383](https://github.com/scipy/scipy/pull/10383) (Thanks [@peterbell10](https://github.com/peterbell10)!)

- [ ] `scipy.ndimage` [PR #14356](https://github.com/scipy/scipy/pull/14356) 
  - [ ] Filters
  - [ ] Fourier filters
  - [ ] Interpolation
  - [ ] Measurements
  - [ ] Morphology

- [ ] `scipy.linalg` [PR #14407](https://github.com/scipy/scipy/pull/14407) (Thanks [@czgdp1807](https://github.com/czgdp1807)!)

- [ ] `scipy.special`


{{% note %}}
SciPy currently supports this through the `scipy.fft` module.
There are other scipy modules that will benefit from `uarray` backend, and later extending the usage through libraries like `CuPy` ([cupyx.scipy](https://github.com/cupy/cupy/tree/master/cupyx/scipy)) and `Dask` ([dask.array](https://github.com/dask/dask/tree/main/dask/array)) etc.
{{% /note %}}

---

SciPy uarray ndimage PR: https://github.com/scipy/scipy/pull/14356

```python{1-3|5|5-7}
# SciPy ndimage with CuPy array
from scipy import ndimage
import cupy as cp

with scipy.ndimage.set_backend('cupy'):
    y_cupy = ndimage.correlate1d(cp.arange(10),
                                 cp.array([1, 2.5]))
```

---

# uarray Demo: SciPy with CuPy Arrays

---

# Thanks!

{{% grid middle %}}

{{< g 1 >}}
{{< figure src="images/quansight_logo_white.png" height="280px" >}}


{{< /g >}}

{{< g 1 >}}

Anirudh Dagar
{{< social >}}
{{< talk-link qs-intern-talk >}}

{{< /g >}}

{{% /grid %}}

{{% note %}}
Special mention to my very supportive mentor Ralf Gommers (@rgommers) for making everything possible since day 1.
I couldn't have asked for more, got lucky with a mentor like Ralf. Thank you so very much.

Melissa (@melissawm) and the whole team behind the internship program.

Gregory R. Lee (@grlee77) and Peter Bell (@peterbell10) for the guidance with ``uarray``.
Whole PyTorch team who have been super super helpful in sharing their knowledge and insights on contributing to PyTorch.

I've mentioned this several times in the past, I plan to continue my open source journey in SciPy and PyTorch and hopefully
keep collaborating with the amazing folks in Quansight.
{{% /note %}}
