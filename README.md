# Template_Docs
Template for Documentation Projects

## 🚀 How to publish your docs with MkDocs Material and GitHub Pages

1. **Push your changes to GitHub**
   - All documentation lives in the `docs/` folder as Markdown files.
   - The site configuration is in `mkdocs.yml`.

2. **Enable GitHub Pages**
   - Go to your repository on GitHub.
   - Click on `Settings` > `Pages`.
   - Set the source branch to `gh-pages` (created automatically by the workflow).
   - Save your changes.

3. **Automatic Deployment**
   - Every push to the `main` branch will trigger a build and deploy your site to GitHub Pages using the workflow in `.github/workflows/deploy.yml`.
   - Your site will be available at: `https://<your-github-username>.github.io/<repository-name>/`

4. **Edit your docs**
   - Update or add Markdown files in the `docs/` folder.
   - Edit `mkdocs.yml` to change navigation, theme, or site settings.
   - Commit and push your changes to update the site.

## 📝 Example Docs Structure
```
Template_MkDocs/
├── docs/
│   ├── index.md
│   └── about.md
├── mkdocs.yml
└── .github/
    └── workflows/
        └── deploy.yml
```

## 📚 Resources
- [MkDocs Material Documentation](https://squidfunk.github.io/mkdocs-material/)
- [MkDocs User Guide](https://www.mkdocs.org/user-guide/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)

---
No local installation required. Everything builds and deploys in the cloud for free!
