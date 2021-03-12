<script>
  function scrape(node, params) {
    function listener() {
      const links = Array.from(document.querySelectorAll(".title-link a"));
      const rows = links.map((l) => [l.textContent, l.href]);
      const data = [["title", "url"], ...rows];
      const text = data
        .map((row) => row.map((col) => `"${col.replaceAll('"', '""')}"`).join(","))
        .join("\n");
      const blob = new Blob([text], { type: "text/csv" });
      const url = URL.createObjectURL(blob);
      node.href = url;
    }
    node.addEventListener("mouseenter", listener);
    return () => node.removeEventListener(listener);
  }
</script>

<div class="userscript">
  <a use:scrape download="data.csv">
    <button>Scrape It!</button>
  </a>
</div>

<style>
  .userscript {
    position: fixed;
    top: 0;
    right: 0;
  }
</style>
