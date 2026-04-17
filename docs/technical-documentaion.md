# Technical Documentation  
Personal Portfolio Assignment

---

## 1. Project Overview

This project is a personal portfolio website developed to present my academic background, technical skills, and completed projects in a clear and organized way.  

The website is implemented as a static web application that runs entirely on the client side without requiring a traditional backend or database.

In this assignment, the project was extended to include more advanced features such as API integration, complex logic, state management, and performance optimization.

The main objective of this assignment is to enhance interactivity and improve overall user experience.

---

## 2. Project Scope

- Front-end development only  
- Technologies used:
  - HTML for page structure
  - CSS for styling, layout, and animations
  - JavaScript for interactivity, state management, and API integration
- External API integration (GitHub API)
- No custom backend or database
- Optimized for desktop and mobile browsers
- Performance improvements using PageSpeed Insights

---

## 3. Project Structure

The project follows a clean and organized folder structure that separates content, styling, logic, and documentation to improve maintainability.

```
202279560-Alanoud-Aldaej-assignment-2/
├── README.md
├── index.html
├── css/
│   └── styles.css
├── js/
│   └── script.js
├── assets/
│   └── images/
├── docs/
    ├── ai-usage-report.md
    └── technical-documentation.md
    └── Demo1.mp4
    └── Demo2.mp4

```

---

## 4. Main Website Sections

The portfolio consists of the following main sections:

- Navigation Bar  
- Hero Section  
- Greeting Section (Dynamic User Welcome)  
- About Section  
- Projects Section (Dynamic from API)  
- Featured Projects Section  
- Skills Section  
- Contact Section  

Each section is implemented using semantic HTML elements to enhance readability and accessibility.

The **Greeting** Section displays a personalized message based on the user's stored name using localStorage.

The **Projects** Section retrieves repositories dynamically from the GitHub API, while the Featured Projects Section highlights selected projects to improve visibility and user focus.

---

## 5. Implemented Features

### 5.1 API Integration (GitHub Repositories)

The Projects section dynamically fetches data from the GitHub API to display repositories.

This allows the website to automatically update project content without manually editing HTML.

The API response is processed using JavaScript and rendered dynamically into project cards.

This demonstrates:
- Fetch API usage  
- JSON data handling  
- Dynamic DOM rendering  

---

### 5.2 Complex Logic (Filtering and Sorting)

Additional logic was implemented to enhance how projects are displayed:

- Filtering repositories based on specific conditions  
- Sorting repositories dynamically  
- Selecting featured projects from the API data  

This improves usability by organizing content in a meaningful way.

---

### 5.3 State Management using Local Storage

The project uses localStorage to store user-related data such as:

- Dark/Light theme preference  
- Visitor name (used in the Greeting Section)

This ensures that user preferences persist even after refreshing or reopening the page.

---

### 5.4 Dynamic User Interaction (Greeting Feature)

Users can enter their name, which is saved and displayed as a personalized greeting message when they revisit the site.

This feature enhances user engagement and creates a more personalized experience.

---
### 5.5 Performance Optimization

The website performance was tested using PageSpeed Insights.

Based on the results, several improvements were applied:

- Adding width and height attributes to images  
- Reducing unused CSS  
- Improving image delivery  
- Fixing render-blocking resources  
- Improving caching efficiency
After applying these optimizations, the performance score improved significantly.

---

## 6. Use of AI Tools

AI tools were used as supportive learning aids rather than direct solutions.

They were mainly used for:

- Assisting with API integration (GitHub API)  
- Helping implement complex logic (filtering and sorting)  
- Debugging HTML, CSS, and JavaScript issues (such as non-working buttons and layout problems)  
- Explaining performance optimization techniques  
- Supporting understanding of state management using localStorage  

All AI-generated suggestions were reviewed and modified before being used.  
The final implementation decisions were made independently.

---

## 7. Learning Outcomes

Through this assignment, I learned how to:

- Implement API integration using JavaScript (GitHub API)  
- Apply complex logic such as filtering and sorting  
- Manage application state using localStorage  
- Improve website performance using PageSpeed Insights  
- Enhance user experience through dynamic and interactive features
