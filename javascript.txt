const searchInput = document.getElementById("search-input");
const resultsTable = document.getElementById("results-table");
const resultsBody = document.getElementById("results-body");
const loader = document.getElementById("loader");
const errorMessage = document.getElementById("error-message");

async function searchSongs() {
  const term = searchInput.value.trim();

  // Prikazujemo loader dok se podaci preuzimaju
  loader.style.display = "block";
  errorMessage.textContent = "";
  resultsBody.innerHTML = "";

  if (term === "") {
    loader.style.display = "none";
    return;
  }

  try {
    const response = await fetch(`https://itunes.apple.com/search?term=${term}&entity=song`);
    const data = await response.json();

    // Skrivanje loadera nakon preuzimanja podataka
    loader.style.display = "none";

    if (data.results.length === 0) {
      errorMessage.textContent = "No results found.";
      return;
    }

    // Popunjavanje tabele rezultatima
    data.results.forEach((result) => {
      const row = document.createElement("tr");
      const songCell = document.createElement("td");
      const artistCell = document.createElement("td");

      songCell.textContent = result.trackName;
      artistCell.textContent = result.artistName;

      row.appendChild(songCell);
      row.appendChild(artistCell);
      resultsBody.appendChild(row);
    });
  } catch (error) {
    loader.style.display = "none";
    errorMessage.textContent = "An error occurred while fetching the data.";
    console.error(error);
  }
}
