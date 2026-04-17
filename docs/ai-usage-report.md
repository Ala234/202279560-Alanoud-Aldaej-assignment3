# Tools Used & Use Cases

During this assignment, I used one AI tool primarily as a supportive tool to help me understand concepts, improve user experience, and solve small implementation issues, while all final implementation and design decisions were my own.

## `ChatGPT`

ChatGPT was used as the main AI assistant during the project. Its use cases included:

- **Code generation & debugging:**  
I used ChatGPT to help debug HTML, CSS, and JavaScript issues, such as theme switching logic, layout issues, and minor implementation problems.

- **Code review & explanations:**  
ChatGPT helped explain some HTML, CSS, and JavaScript concepts, especially how interactive features work and how different parts of the code connect together.

- **Documentation support:**  
It assisted in improving the clarity and organization of documentation and project explanation.

- **How to enhance user experience:**  
ChatGPT was used to help me understand how to improve user experience through simple interactive features such as navigation, theme switching, transitions, and form validation.

---

# Detailed AI Usage

## 1. API Integration

**HTML**
```HTML
<!-- API Section -->
<section id="github" class="section">
  <h2>GitHub Repositories </h2>
  <div id="repo-container" class="projects-grid"></div>
  <p id="api-message"></p>
</section>  
```
**JavaScript**
```javascript
// ===== GitHub API Integration =====

const repoContainer = document.getElementById("repo-container");
const apiMessage = document.getElementById("api-message");

fetch("https://api.github.com/users/ala234/repos")
  .then(response => {
    if (!response.ok) {
      throw new Error("API failed");
    }
    return response.json();
  })
  .then(data => {
    repoContainer.innerHTML = "";

    data.slice(0, 6).forEach(repo => {
      const card = document.createElement("div");
      card.classList.add("project-card");

      card.innerHTML = `
        <h3>${repo.name}</h3>
        <p>${repo.description || "No description available"}</p>
        <a href="${repo.html_url}" target="_blank">View Project</a>
      `;

      repoContainer.appendChild(card);
    });
  })
  .catch(error => {
    apiMessage.textContent = "⚠️ Failed to load GitHub projects. Please try again later.";
    console.error(error);
  });
```
<img width="1280" height="340" alt="image" src="https://github.com/user-attachments/assets/0250fe3e-19bc-4725-b2c8-6623fc772d5a" />



> In  this part, I used AI assistance to implement GitHub API integration for displaying repositories. The initial AI-generated output was unstructured, so I improved it by reorganizing the layout and enhancing the design to fit the portfolio style.

## Applying Modification:

**HTML**
```HTML
<!-- API Section -->
<section id="github" class="section">
  <h2>Featured Projects</h2>
  <div id="repo-container" class="projects-grid"></div>
  <p id="api-message"></p>
</section>
```
**JavaScript**
```javascript
// ===== GitHub API Integration =====

const repoContainer = document.getElementById("repo-container");
const apiMessage = document.getElementById("api-message");

fetch("https://api.github.com/users/Ala234/repos")
  .then(response => {
    if (!response.ok) {
      throw new Error("API failed");
    }
    return response.json();
  })
  .then(data => {
    repoContainer.innerHTML = "";
    apiMessage.textContent = "";

    const filteredRepos = data
      .filter(repo =>
        !repo.name.toLowerCase().includes("assignment") &&
        !repo.fork
      )
      .slice(0, 4);

    filteredRepos.forEach(repo => {
      const card = document.createElement("div");
      card.classList.add("project-card");

      const shortName =
        repo.name.length > 25
          ? repo.name.substring(0, 25) + "..."
          : repo.name;

      card.innerHTML = `
        <div class="repo-icon">
          <i class="fa-brands fa-github"></i>
        </div>
        <h3>${shortName}</h3>
        <p>${repo.description || "No description available"}</p>
        <a href="${repo.html_url}" target="_blank">View Project</a>
      `;

      repoContainer.appendChild(card);
    });

    if (filteredRepos.length === 0) {
      apiMessage.textContent = "No featured GitHub projects found right now.";
    }
  })
  .catch(error => {
    apiMessage.textContent = "⚠️ Failed to load featured projects. Please try again later.";
    console.error(error);
  });
```
**CSS**
```CSS
/* ===== GitHub API Section ===== */
#api-message {
  margin-top: 1rem;
  font-size: 1rem;
  color: #fca5a5;
}

.project-card a {
  display: inline-block;
  margin-bottom: 1rem;
  color: #60a5fa;
  text-decoration: none;
  font-weight: bold;
}

.project-card a:hover {
  text-decoration: underline;
}

.repo-icon {
  margin-top: 1rem;
  font-size: 2rem;
  color: #c77dff;
}
```
<img width="1280" height="441" alt="image" src="https://github.com/user-attachments/assets/83e8a3ca-efea-4617-8197-b04c97f48068" />



>This is the layout After modifying AI's generated Code

---

## 2. Complex Logic

**1) Project List Filtering and Sorting**

**HTML**
```html
<!--  PROJECTS  -->
<section id="projects" class="section">
  <h2>Projects</h2>

  <div class="project-controls">
    <button class="filter-btn" data-filter="all">All</button>
    <button class="filter-btn" data-filter="web">Web</button>
    <button class="filter-btn" data-filter="mobile">Mobile</button>

    <label for="sort-projects" class="visually-hidden">Sort:</label>
    <select id="sort-projects">
      <option value="default">Sort By</option>
      <option value="name-asc">Name A-Z</option>
      <option value="name-desc">Name Z-A</option>
    </select>
  </div>

  <div class="projects-grid" id="projects-grid">
    <div class="project-card" data-category="web" data-name="KFUPM Impact Hub">
      <img src="asset/images/impact-hub.jpg" alt="KFUPM Impact Hub" loading="lazy" width="200" height="200">
      <h3>KFUPM Impact Hub</h3>
      <p>Volunteer events and community initiatives platform.</p>
    </div>

    <div class="project-card" data-category="mobile" data-name="My Super Tour Guide">
      <img src="asset/images/super-tour.jpg" alt="My Super Tour Guide"  loading="lazy" width="200" height="200">
      <h3>My Super Tour Guide</h3>
      <p>Tour booking and travel guide mobile application.</p>
    </div>
  </div>
</section>
```
**JavaScript**
```javascript
// ===== Project Filter and Sort =====

const filterButtons = document.querySelectorAll(".filter-btn");
const sortSelect = document.getElementById("sort-projects");
const projectsGrid = document.getElementById("projects-grid");

if (projectsGrid && filterButtons.length > 0 && sortSelect) {
  const originalCards = Array.from(projectsGrid.querySelectorAll(".project-card"));
  let currentFilter = "all";
  let currentSort = "default";

  function renderProjects() {
    let filteredCards = originalCards.filter(card => {
      const category = card.dataset.category;
      return currentFilter === "all" || category === currentFilter;
    });

    if (currentSort === "name-asc") {
      filteredCards.sort((a, b) =>
        a.dataset.name.localeCompare(b.dataset.name)
      );
    } else if (currentSort === "name-desc") {
      filteredCards.sort((a, b) =>
        b.dataset.name.localeCompare(a.dataset.name)
      );
    }

    projectsGrid.innerHTML = "";
    filteredCards.forEach(card => projectsGrid.appendChild(card));
  }

  filterButtons.forEach(button => {
    button.addEventListener("click", () => {
      currentFilter = button.dataset.filter;
      renderProjects();
    });
  });

  sortSelect.addEventListener("change", () => {
    currentSort = sortSelect.value;
    renderProjects();
  });

  renderProjects();
}
```
**CSS**
```CSS
/* ===== Project Controls ===== */
.project-controls {
  display: flex;
  justify-content: center;
  gap: 1rem;
  flex-wrap: wrap;
  margin-bottom: 2rem;
}

.project-controls button,
.project-controls select {
  padding: 10px 16px;
  border: none;
  border-radius: 10px;
  font-size: 1rem;
  cursor: pointer;
}

.project-controls button {
  background: #4b0bb1;
  color: white;
}

.project-controls select {
  background: white;
  color: #111827;
}
```
<img width="509" height="60" alt="image" src="https://github.com/user-attachments/assets/b61481a3-2248-46ca-8500-047516459df5" />

In this part, I used navigation links to allow users to move between different sections of the portfolio such as About, Projects, Skills, and Contact. This makes the website more interactive and easier to use. AI helped me understand how internal navigation works and how it improves the overall user experience.

---

## 3. Smooth interaction

```css
#theme-toggle {
    transition: 0.3s, transform 0.2s;
}

#theme-toggle:hover {
    background: rgba(255,255,255,0.25);
    transform: scale(1.05);
}

.project-card {
  transition: transform 0.3s ease;
}

.project-card:hover {
  transform: translateY(-6px);
}

.skill {
    transition: transform 0.3s, box-shadow 0.3s;
}

.skill:hover {
    transform: translateY(-8px);
    box-shadow: 0 15px 30px rgba(199,125,255,0.5);
}
```
<img width="1197" height="359" alt="image" src="https://github.com/user-attachments/assets/0788885b-2693-420e-a745-2c1ff8652969" />

In this part, I applied CSS transitions and hover effects to different elements, such as the theme button, project cards, and skill boxes. These effects make interactions feel smoother and give visual feedback when the user hovers over elements. AI helped me understand how transitions work in CSS and how they can be used to enhance user experience without making the design too complex.

## 4. Error handling

```html
<form class="contact-form">
  <input type="text" placeholder="Name" required>
  <input type="email" placeholder="Email" required>
  <textarea placeholder="Message" rows="5" required></textarea>
  <button type="submit">Send Message</button>
</form>
```
<img width="928" height="622" alt="image" src="https://github.com/user-attachments/assets/3da38a5c-6f0b-4d3d-a544-f9172248bdde" />

In this part, I used built-in HTML validation using the `required` attribute to prevent empty inputs. Also, when entering an invalid email format, the browser automatically shows a message asking the user to include a valid format (such as including “@”). This provides basic error handling and improves the usability of the form. AI helped me understand how validation works and why it is important for user input.

---

## Benefits

- Improved understanding of how interactive features are implemented.
- Learned how to use `localStorage` to store user preferences.
- Better understanding of navigation and user interaction.
- Improved ability to apply transitions and enhance UI responsiveness.
- Learned basic form validation and error handling.

---

## Challenges

- Some AI suggestions needed adjustment to fit the project.
- Not all suggested solutions were directly usable.
- Required reviewing and modifying code to match assignment requirements.

---

## Learning Outcomes

Through using AI tools in this assignment, I learned:

- How to implement theme switching using JavaScript and CSS.
- How to store and retrieve data using `localStorage`.
- How navigation improves usability in a portfolio.
- How transitions enhance interaction.
- How basic validation supports error handling.

---

## Responsible Use & Modifications

All AI-generated suggestions were reviewed and modified before use.

I ensured:

- The final implementation was my own work.
- AI was used only for support and understanding.
- Suggestions were adapted, not copied directly.
- The project maintains originality and academic integrity.
