---
layout: default
title: CPU Leaderboard
---

# CPU Leaderboard (PassMark CPU Mark)

<p>
  <label for="cpuSearch">Search:</label>
  <input id="cpuSearch" type="search" placeholder="e.g. 7800X3D, i7-13700K" style="min-width: 280px;" />
</p>

<table id="cpuTable">
  <thead>
    <tr>
      <th data-sort="name">Name</th>
      <th data-sort="brand">Brand</th>
      <th data-sort="platform">Platform</th>
      <th data-sort="cpu_mark" data-sort-type="number">CPU Mark</th>
      <th data-sort="cores" data-sort-type="number">Cores</th>
      <th data-sort="threads" data-sort-type="number">Threads</th>
      <th>Source</th>
    </tr>
  </thead>
  <tbody>
    {% assign sorted = site.data.cpus | sort: "cpu_mark" | reverse %}
    {% for cpu in sorted %}
      <tr>
        <td class="col-name">{{ cpu.name }}</td>
        <td class="col-brand">{{ cpu.brand }}</td>
        <td class="col-platform">{{ cpu.platform }}</td>
        <td class="col-cpu_mark">{{ cpu.cpu_mark }}</td>
        <td class="col-cores">{{ cpu.cores }}</td>
        <td class="col-threads">{{ cpu.threads }}</td>
        <td>
          {% if cpu.source_url and cpu.source_url != "" %}
            <a href="{{ cpu.source_url }}" target="_blank" rel="noopener">link</a>
          {% else %}
            -
          {% endif %}
        </td>
      </tr>
    {% endfor %}
  </tbody>
</table>

<script>
(function () {
  const input = document.getElementById("cpuSearch");
  const table = document.getElementById("cpuTable");
  const tbody = table.querySelector("tbody");
  const headers = Array.from(table.querySelectorAll("thead th[data-sort]"));

  function getCellValue(row, key) {
    const el = row.querySelector(".col-" + key);
    return (el ? el.textContent : "").trim();
  }

  function normalize(s) {
    return (s || "").toLowerCase();
  }

  // Search filter
  input.addEventListener("input", () => {
    const q = normalize(input.value);
    const rows = Array.from(tbody.querySelectorAll("tr"));
    rows.forEach(row => {
      const text = normalize(row.textContent);
      row.style.display = text.includes(q) ? "" : "none";
    });
  });

  // Sorting
  let currentSort = { key: "cpu_mark", dir: "desc", type: "number" };

  function parseValue(val, type) {
    if (type === "number") {
      const n = Number(String(val).replace(/[^\d.-]/g, ""));
      return Number.isFinite(n) ? n : -Infinity;
    }
    return normalize(val);
  }

  function sortBy(key, type) {
    const rows = Array.from(tbody.querySelectorAll("tr"));

    if (currentSort.key === key) {
      currentSort.dir = currentSort.dir === "asc" ? "desc" : "asc";
    } else {
      currentSort.key = key;
      currentSort.dir = (type === "number") ? "desc" : "asc";
    }
    currentSort.type = type || "text";

    rows.sort((a, b) => {
      const av = parseValue(getCellValue(a, key), currentSort.type);
      const bv = parseValue(getCellValue(b, key), currentSort.type);

      if (av < bv) return currentSort.dir === "asc" ? -1 : 1;
      if (av > bv) return currentSort.dir === "asc" ? 1 : -1;
      return 0;
    });

    // Re-append rows in new order
    rows.forEach(r => tbody.appendChild(r));
  }

  headers.forEach(th => {
    const key = th.getAttribute("data-sort");
    const type = th.getAttribute("data-sort-type") || "text";
    th.style.cursor = "pointer";
    th.title = "Click to sort";
    th.addEventListener("click", () => sortBy(key, type));
  });
})();
</script>
