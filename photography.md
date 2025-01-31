---
layout: default
title: "Photography"
---

# Photography

<div class="gallery" id="galleryContainer">
  <!-- JavaScript will inject <a><img></a> items here -->
</div>

<script>
  // -- 1) Configure your GitHub user/repo/folder:
  const GITHUB_USER = "YourGitHubUsername";
  const GITHUB_REPO = "YourRepoName";
  const FOLDER_PATH = "assets/images"; // or wherever your images are

  // -- 2) Construct the GitHub API URL for that folder:
  //     https://api.github.com/repos/:owner/:repo/contents/:path
  const apiURL = `https://api.github.com/repos/${GITHUB_USER}/${GITHUB_REPO}/contents/${FOLDER_PATH}`;

  // Container where we'll insert the gallery items
  const galleryContainer = document.getElementById("galleryContainer");

  // Fetch the list of files in the folder
  fetch(apiURL)
    .then((response) => {
      if (!response.ok) {
        throw new Error("Network response was not OK");
      }
      return response.json();
    })
    .then((files) => {
      // 'files' is an array of objects describing each item in the folder
      files.forEach((file) => {
        if (file.type === "file") {
          const lowerName = file.name.toLowerCase();
          // Check if it's an image file
          if (
            lowerName.endsWith(".jpg") ||
            lowerName.endsWith(".jpeg") ||
            lowerName.endsWith(".png") ||
            lowerName.endsWith(".gif")
          ) {
            createGalleryItem(file.download_url, file.name);
          }
        }
      });
    })
    .catch((error) => {
      console.error("Error fetching folder contents:", error);
      galleryContainer.innerHTML =
        "<p style='color:red;'>Error loading images from GitHub. Check console.</p>";
    });

  // -- 3) Helper to create each <a> (lightbox) and <img> and add to container
  function createGalleryItem(imageUrl, imageName) {
    // <a data-lightbox="gallery" data-title="fileName" href="https://...">
    const link = document.createElement("a");
    link.href = imageUrl;
    link.setAttribute("data-lightbox", "mygallery");
    link.setAttribute("data-title", imageName);

    // <img src="https://..." alt="fileName" />
    const img = document.createElement("img");
    img.src = imageUrl;
    img.alt = imageName;
    // Optionally add inline styles or classes

    link.appendChild(img);
    galleryContainer.appendChild(link);
  }
</script>
