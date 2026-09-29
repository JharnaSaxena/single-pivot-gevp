<!DOCTYPE html>
<html lang="en">

<body>

<h1>Single-Pivot GEVP Analysis</h1>
<p class="header-meta">Single-pivot generalized eigenvalue problem analysis of CoSMoN C103 lattice QCD correlator data for the nucleon, dineutron, and deuteron channels.</p>

<h2>Overview</h2>
<p>This repository implements the single-pivot GEVP pipeline described in:</p>
<ul>
  <li>Sarah Skinner's PhD thesis, Section 4.2 and Eqs. (4.9)–(4.12)</li>
  <li>Paper <a href="https://arxiv.org/abs/2505.05547">arXiv:2505.05547</a>, Section II.C and Eqs. (2.1)–(2.21)</li>
</ul>

<p>The workflow is:</p>
<ol>
  <li>Load the correlator matrix <code>C_ij(t)</code> from the HDF5 file</li>
  <li>Symmetrize <code>C(t)</code> to enforce exact Hermiticity</li>
  <li>Solve the GEVP <code>C(t_D) v_n = λ_n C(t_0) v_n</code> at <code>t_0 = 5</code>, <code>t_D = 10</code></li>
  <li>Order eigenvalues <code>λ_0 ≥ λ_1 ≥ ...</code></li>
  <li>Normalize <code>V† C(t_0) V = I</code></li>
  <li>Rotate <code>D(t) = V† C(t) V</code> at all <code>t</code> and enforce Hermiticity after rotation</li>
  <li>Compute effective masses <code>m_eff(t_s) = ln(C(t_s − 0.5) / C(t_s + 0.5))</code></li>
  <li>Bootstrap 1000 resamples with the same fixed pivot from the mean</li>
  <li>Extract the ROT0 plateau over <code>t_s ∈ [4.5, 8.5]</code></li>
  <li>Perform stability scans and single-exp / constant fits</li>
</ol>

<h2>Requirements</h2>
<pre><code>numpy
scipy
h5py
matplotlib</code></pre>
<p>Install with:</p>
<pre><code>pip install -r requirements.txt</code></pre>

<h2>Data</h2>
<p>The three HDF5 files are not included in this repository. They are available from the CoSMoN collaboration at:</p>
<pre><code>https://portal.nersc.gov/cfs/m2986/cosmon/nn_c103_2505.05547/cosmon_c103_r005-8_nucleon.hdf5
https://portal.nersc.gov/cfs/m2986/cosmon/nn_c103_2505.05547/cosmon_c103_r005-8_dineutron_Swave.hdf5
https://portal.nersc.gov/cfs/m2986/cosmon/nn_c103_2505.05547/cosmon_c103_r005-8_deuteron_Swave.hdf5</code></pre>
<p>Each notebook downloads its own file automatically via <code>urllib.request</code>.</p>

<h2>Notebooks</h2>
<table>
  <thead>
    <tr><th>File</th><th>Purpose</th></tr>
  </thead>
  <tbody>
    <tr><td><code>01_nucleon_effective_mass.ipynb</code></td><td>Raw correlator and effective mass for the single nucleon</td></tr>
    <tr><td><code>02_nucleon_stability_and_fits.ipynb</code></td><td>Stability scan and single-exp / constant fits for the nucleon</td></tr>
    <tr><td><code>03_dineutron_gevp.ipynb</code></td><td>GEVP across all five dineutron sectors</td></tr>
    <tr><td><code>04_dineutron_stability_and_fits.ipynb</code></td><td>Stability scan and fits for <code>PSQ0_A1g</code></td></tr>
    <tr><td><code>05_deuteron_gevp.ipynb</code></td><td>GEVP across all ten deuteron irreps + irrep averaging</td></tr>
    <tr><td><code>06_deuteron_stability_and_fits.ipynb</code></td><td>Stability scan and fits for <code>PSQ0_T1g</code></td></tr>
  </tbody>
</table>

<h2>Key results</h2>

<h3>Single nucleon</h3>
<ul>
  <li><code>m_eff(t_s = 2.5) = 0.79451 ± 0.00024</code> (G1g_1), <code>0.79445 ± 0.00024</code> (G1g_2)</li>
  <li>Paper Fig. 1 range: 0.75–0.83. Both inside.</li>
</ul>

<h3>Dineutron (t_0 = 5, t_D = 10)</h3>
<table>
  <thead>
    <tr><th>Sector</th><th>N_op</th><th>ξ_cn</th><th>E_0</th><th>σ</th></tr>
  </thead>
  <tbody>
    <tr><td>PSQ0_A1g</td><td>6</td><td>1.818</td><td>1.430475</td><td>0.000271</td></tr>
    <tr><td>PSQ1_A1</td><td>10</td><td>1.600</td><td>1.441928</td><td>0.000274</td></tr>
    <tr><td>PSQ2_A1</td><td>21</td><td>1.592</td><td>1.453540</td><td>0.000265</td></tr>
    <tr><td>PSQ3_A1</td><td>9</td><td>1.280</td><td>1.465057</td><td>0.000266</td></tr>
    <tr><td>PSQ4_A1</td><td>10</td><td>1.585</td><td>1.453877</td><td>0.000275</td></tr>
  </tbody>
</table>

<h3>Deuteron (t_0 = 5, t_D = 10)</h3>
<table>
  <thead>
    <tr><th>Sector</th><th>N_op</th><th>ξ_cn</th><th>E_0</th><th>σ</th></tr>
  </thead>
  <tbody>
    <tr><td>PSQ0_T1g</td><td>15</td><td>1.802</td><td>1.430171</td><td>0.000298</td></tr>
    <tr><td>PSQ1_A2</td><td>10</td><td>1.599</td><td>1.441164</td><td>0.000305</td></tr>
    <tr><td>PSQ1_E</td><td>18</td><td>1.616</td><td>1.441565</td><td>0.000294</td></tr>
    <tr><td>PSQ2_A2</td><td>15</td><td>1.597</td><td>1.452531</td><td>0.000297</td></tr>
    <tr><td>PSQ2_B1</td><td>19</td><td>1.610</td><td>1.452603</td><td>0.000298</td></tr>
    <tr><td>PSQ2_B2</td><td>21</td><td>1.599</td><td>1.453297</td><td>0.000296</td></tr>
    <tr><td>PSQ3_A2</td><td>9</td><td>1.275</td><td>1.463999</td><td>0.000288</td></tr>
    <tr><td>PSQ3_E</td><td>17</td><td>1.304</td><td>1.463699</td><td>0.000301</td></tr>
    <tr><td>PSQ4_A2</td><td>7</td><td>1.585</td><td>1.453593</td><td>0.000282</td></tr>
    <tr><td>PSQ4_E</td><td>15</td><td>1.593</td><td>1.453600</td><td>0.000298</td></tr>
  </tbody>
</table>

<p>All energies are above the non-interacting two-nucleon threshold <code>2 a m_N = 1.40534</code>, consistent with the paper's no-bound-state conclusion.</p>

<h2>Methodology notes</h2>
<ul>
  <li><strong>Fixed pivot</strong>: the GEVP is solved once on the mean; the same <code>V</code> is applied to all bootstrap resamples. This follows Sarah Sec. 4.2 and paper Sec. II.C.</li>
  <li><strong>Hermiticity after rotation</strong>: <code>D(t) → ½(D(t) + D(t)†)</code> is enforced after every rotation. The anti-Hermitian residual drops from ~1e-10 to exactly 0.0 at double precision.</li>
  <li><strong>Amplitude note</strong>: the single-exp fit returns <code>A ≈ 1250</code>. This is a metric-normalization artifact — the GEVP convention <code>V† C(t_0) V = I</code> forces <code>D_00(t_0) = 1</code>, so <code>A = exp(E_0 t_0)</code>. The physics is in <code>E_0</code>, not <code>A</code>.</li>
</ul>

<h2>Not in scope</h2>
<ul>
  <li>lab-to-CM conversion</li>
  <li>QC2 / q cot δ / ERE fit</li>
  <li>moving (rolling) pivot</li>
  <li>Lanczos noise reduction</li>
  <li>head-to-head comparison against a Sigmond production run on the same HDF5</li>
</ul>

<h2>Citation</h2>
<p>If using this code, cite:</p>
<ul>
  <li>Sarah Skinner, PhD thesis</li>
  <li><a href="https://arxiv.org/abs/2505.05547">arXiv:2505.05547</a></li>
</ul>

<h2>Author</h2>
<p>Jharna</p>

<div class="footer">
  &copy; 2026 · Single-Pivot GEVP Analysis
</div>

</body>
</html>
