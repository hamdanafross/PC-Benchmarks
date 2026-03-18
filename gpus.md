---
layout: default
title: GPU Leaderboard
---

# GPU Leaderboard (PassMark G3D Mark)

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Brand</th>
      <th>Score (G3D Mark)</th>
      <th>VRAM (GB)</th>
      <th>TDP (W)</th>
    </tr>
  </thead>
  <tbody>
    {% assign sorted = site.data.gpus | sort: "g3d_mark" | reverse %}
    {% for gpu in sorted %}
      <tr>
        <td>{{ gpu.name }}</td>
        <td>{{ gpu.brand }}</td>
        <td>{{ gpu.g3d_mark }}</td>
        <td>{{ gpu.vram_gb }}</td>
        <td>{{ gpu.tdp_w }}</td>
      </tr>
    {% endfor %}
  </tbody>
</table>
