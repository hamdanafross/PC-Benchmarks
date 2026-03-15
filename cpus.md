---
layout: default
title: CPU Leaderboard
---

# CPU Leaderboard (PassMark CPU Mark)

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Brand</th>
      <th>Score (CPU Mark)</th>
      <th>Cores</th>
      <th>Threads</th>
    </tr>
  </thead>
  <tbody>
    {% assign sorted = site.data.cpus | sort: "cpu_mark" | reverse %}
    {% for cpu in sorted %}
      <tr>
        <td>{{ cpu.name }}</td>
        <td>{{ cpu.brand }}</td>
        <td>{{ cpu.cpu_mark }}</td>
        <td>{{ cpu.cores }}</td>
        <td>{{ cpu.threads }}</td>
      </tr>
    {% endfor %}
  </tbody>
</table>
