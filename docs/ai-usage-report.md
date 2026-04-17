# Tools Used & Use Cases

During this assignment, I used one AI tool primarily as a supportive tool to help me understand concepts, improve user experience, and solve small implementation issues, while all final implementation and design decisions were my own.

## `ChatGPT`

ChatGPT was used as the main AI assistant during the project. Its use cases included:

- **Code generation & debugging:**
I used ChatGPT to help debug HTML, CSS, and JavaScript issues, such as non-working buttons, layout problems, and fixing script errors. It also helped in troubleshooting issues related to API integration and improving functionality.

- **Code review & explanations:**  
ChatGPT helped explain HTML, CSS, and JavaScript concepts, especially related to API integration, complex logic (such as filtering and sorting), and performance optimization. It clarified how different parts of the code work together.

- **Documentation support:**  
It assisted in improving the clarity and organization of documentation and project explanation.

- **How to enhance user experience:**  
ChatGPT was used to understand how to improve user experience through interactive features such as filtering, dynamic content rendering, and smoother UI behavior.

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
[Watch Demo](https://raw.githubusercontent.com/Ala234/202279560-Alanoud-Aldaej-assignment3/refs/heads/main/docs/Demo%201.mp4)

In this part, AI was used to assist with implementing the filtering functionality. The initial output had usability issues (buttons were not responsive), so I revised and improved the code to ensure proper functionality.

 ## Applying Modification:

 **JavaScript**
```javascript
  filterButtons.forEach(button => {
    button.addEventListener("click", () => {
      filterButtons.forEach(btn => btn.classList.remove("active"));
      button.classList.add("active");

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
```css
.filter-btn.active {
  outline: 2px solid white;
  transform: scale(1.05);
}
```
[Watch Demo](https://raw.githubusercontent.com/Ala234/202279560-Alanoud-Aldaej-assignment3/refs/heads/main/docs/Demo%202.mp4)

>This is the layout After modifying AI's generated Code
---

## 3. State Management
**HTML**
```HTML
<section id="visitor-name-section" class="section">
  <h2>Welcome</h2>

  <div class="name-box">
    <input type="text" id="visitor-name-input" placeholder="Enter your name">
    <button id="save-name-btn">Save Name</button>
    <button id="clear-name-btn">Clear</button>
  </div>

  <p id="visitor-greeting">Welcome, guest!</p>
</section>
```
**JavaScript**
```javascript
// ===== Visitor Name State Management =====

const visitorNameInput = document.getElementById("visitor-name-input");
const saveNameBtn = document.getElementById("save-name-btn");
const clearNameBtn = document.getElementById("clear-name-btn");
const visitorGreeting = document.getElementById("visitor-greeting");

if (visitorGreeting) {
  const savedName = localStorage.getItem("visitorName");

  if (savedName) {
    visitorGreeting.textContent = `Welcome, ${savedName}!`;
    if (visitorNameInput) {
      visitorNameInput.value = savedName;
    }
  }

  if (saveNameBtn) {
    saveNameBtn.addEventListener("click", () => {
      const name = visitorNameInput.value.trim();

      if (name === "") {
        visitorGreeting.textContent = "Please enter your name first.";
        visitorGreeting.style.color = "#f87171";
        return;
      }

      localStorage.setItem("visitorName", name);
      visitorGreeting.textContent = `Welcome, ${name}!`;
      visitorGreeting.style.color = "#ffffff";
    });
  }

  if (clearNameBtn) {
    clearNameBtn.addEventListener("click", () => {
      localStorage.removeItem("visitorName");
      visitorNameInput.value = "";
      visitorGreeting.textContent = "Welcome, guest!";
      visitorGreeting.style.color = "#ffffff";
    });
  }
}
```
**CSS**
```CSS
/* ===== Visitor Name Section ===== */
.name-box {
  display: flex;
  justify-content: center;
  gap: 12px;
  flex-wrap: wrap;
  margin-top: 20px;
  margin-bottom: 20px;
}

.name-box input {
  padding: 12px;
  border-radius: 10px;
  border: none;
  outline: none;
  font-size: 1rem;
  min-width: 220px;
}

.name-box button {
  padding: 12px 18px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-size: 1rem;
  background: #4b0bb1;
  color: white;
}

.name-box button:hover {
  opacity: 0.9;
}

#visitor-greeting {
  font-size: 1.1rem;
  font-weight: bold;
  margin-top: 10px;
}
```

<img width="722" height="276" alt="image" src="https://github.com/user-attachments/assets/a8912264-2791-4074-8315-9a079347334a" />


In this part, AI helped me in implementing state management by storing and displaying the visitor’s name using localStorage. The website remembers the user’s name after refresh and allows clearing it when needed.

## 4. Performance
In this part, I evaluated my website using PageSpeed Insights. The initial performance score was not optimal, so I applied several optimizations with AI advices to improve speed and efficiency.

**Adding ```media="all"```**
```html
    <link rel="stylesheet" href="css/styles.css" media="all">
    <!-- for icons ( I used AI help here) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css" media="All">
</head>
```
**Adding ```loading="lazy"``` to imegas**
```html
  <img src="asset/images/impact-hub.jpg" alt="KFUPM Impact Hub" loading="lazy" width="200" height="200">
  <img src="asset/images/super-tour.jpg" alt="My Super Tour Guide"  loading="lazy" width="200" height="200">
```
**Adding ```descriptions``` to meta**
```html
<meta name="description" content="Alanoud Aldaej's Portfolio" >
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
<img width="928" height="622" alt="image" src="https://github.com/user-attachments/assets/3da38a5c-6f0b-4d3d-a544-f9172248bdde" />
```
**Adding label to ```select```**
```html
<label for="sort-projects" class="visually-hidden">Sort:</label>
    <select id="sort-projects">
      <option value="default">Sort By</option>
      <option value="name-asc">Name A-Z</option>
      <option value="name-desc">Name Z-A</option>
    </select>
  </div>
```

<img width="1360" height="662" alt="image" src="https://github.com/user-attachments/assets/05084fd3-b02d-4235-ab02-2573e27cbfee" />

>After re-testing, the performance score improved significantly.
---

## Benefits

- Improved understanding of how interactive features are implemented.
- Learned how to use `localStorage` to store user preferences.
- Better understanding of navigation and user interaction.
- Improved ability to apply transitions and enhance UI responsiveness.
- Learned basic form validation and error handling.

---

## Challenges

- Some AI-generated code did not work correctly at first (e.g., filter buttons were not responsive).
- The initial layout and structure were not well organized and needed adjustments.
- Some suggestions required modification to fit the project requirements and improve usability.

---

## Learning Outcomes

Through using AI tools in this assignment, I learned:

-How to implement API integration (e.g., fetching and displaying GitHub repositories).
-How to build complex logic such as filtering and sorting functionality using JavaScript.
-How to debug HTML, CSS, and JavaScript issues effectively.
-How to improve website performance and understand why performance issues occur.
-How to enhance user experience by making the interface more interactive and responsive.

---

## Responsible Use & Modifications

All AI-generated suggestions were reviewed and modified before use.

I ensured:

- The final implementation was my own work.
- AI was used only for support and understanding.
- Suggestions were adapted, not copied directly.
- The project maintains originality and academic integrity.
