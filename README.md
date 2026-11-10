# Vortex Lab

Documentation and resources for **Vortex Lab**.

## 📖 Documentation Site

The project documentation is built with [Jekyll](https://jekyllrb.com/) using the [Just the Docs](https://just-the-docs.github.io/just-the-docs/) theme and deployed via [GitHub Pages](https://pages.github.com/):

👉 **[https://shphschool.github.io/vortex_lab/](https://shphschool.github.io/vortex_lab/)**

---

## 🛠 Local Development

To run and preview the documentation site locally:

1. Navigate to the `docs` directory:
   ```bash
   cd docs
   ```

2. Install Ruby gems:
   ```bash
   bundle install
   ```

3. Start the Jekyll local server:
   ```bash
   bundle exec jekyll serve
   ```

4. Open `http://localhost:4000/vortex_lab/` in your browser.

---

## 🚀 GitHub Pages Deployment

The documentation site is automatically built and deployed via GitHub Actions whenever changes are pushed to the `main` branch (see `.github/workflows/pages.yml`).

### One-time Setup in GitHub Repository Settings:
1. Open your repository on GitHub: `https://github.com/shphschool/vortex_lab`.
2. Go to **Settings** > **Pages**.
3. Under **Build and deployment** > **Source**, select **GitHub Actions**.
