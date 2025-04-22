---
title: "Recent Publications"
---

<div style="text-align: center;">
  <img src="/img/publications.jpg" alt="Publications" style="max-width: 100%; height: auto;" />
  <p style="font-size: 0.8em;">
    <a href="https://www.vecteezy.com/free-photos/book target="_blank" rel="noopener noreferrer">Book Stock photos by Vecteezy</a>
  </p>
</div>

<div id="orcid-publications">
  <ul id="pub-list">Loading publications from ORCID...</ul>
</div>

<script>
const orcidId = "0009-0002-6008-6339";
const pubList = document.getElementById("pub-list");

fetch(`https://pub.orcid.org/v3.0/${orcidId}/works`, {
    headers: {
      Accept: "application/json"
    }
  })
  .then(response => response.json())
  .then(data => {
    pubList.innerHTML = ""; // Clear loading message

    const works = data.group
      .map(group => group["work-summary"][0])
      .sort((a, b) => {
        const da = a["publication-date"];
        const db = b["publication-date"];
        const yA = da?.year?.value || 0;
        const yB = db?.year?.value || 0;
        return yB - yA;
      })
      .slice(0, 15); // Show most recent 10

    works.forEach(summary => {
      const title = summary.title?.title?.value || "Untitled";
      const date = Object.values(summary["publication-date"]).filter(element => element != null).map(element => element?.value).at(0);
      const journal = summary["journal-title"]?.value || "";
      const url = summary["external-ids"]["external-id"]
        ?.find(id => id["external-id-url"])
        ?.["external-id-url"]?.value || "#"; // Fallback to "#" if URL is missing
      const li = document.createElement("li");
      li.innerHTML = `<strong>${title}</strong> (${date}), ${journal} - <a href="${url}" target="_blank" rel="noopener noreferrer">Link</a>`;
      pubList.appendChild(li);
    });
  })
  .catch(error => {
    pubList.innerHTML = "<li>Unable to load publications from ORCID at the moment.</li>";
    console.error(error);
  });
</script>
