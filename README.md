# template-static-page-deploy

Um den Inhalt des Ordners "public" nach jedem Commit automatisch als statische Webseite über GitHub Pages zu veröffentlichen, gehen Sie bitte wie folgt vor:

1. Wählen Sie unter ["Settings / Pages"](../../settings/pages) bei "Build and Deployment" als "Source" den Eintrag "GitHub Actions".
2. Erstellen Sie in Ihrem Repository eine Datei `.github/workflows/static.yml` (auf den Punkt am Anfang des ersten Verzeichnisses achten) mit folgendem Inhalt:

    ```yaml
    # Simple workflow for deploying static content to GitHub Pages
    name: Deploy static content to Pages

    on:
    # Runs on pushes targeting the default branch
    push:
        branches: ["main"]

    # Allows you to run this workflow manually from the Actions tab
    workflow_dispatch:

    # Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
    permissions:
    contents: read
    pages: write
    id-token: write

    # Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
    # However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
    concurrency:
    group: "pages"
    cancel-in-progress: false

    jobs:
    # Single deploy job since we're just deploying
    deploy:
        environment:
        name: github-pages
        url: ${{ steps.deployment.outputs.page_url }}
        runs-on: ubuntu-latest
        steps:
        - name: Checkout
            uses: actions/checkout@v4
        - name: Setup Pages
            uses: actions/configure-pages@v5
        - name: Upload artifact
            uses: actions/upload-pages-artifact@v3
            with:
            # Upload public folder
            path: 'public'
        - name: Deploy to GitHub Pages
            id: deployment
            uses: actions/deploy-pages@v4
    ```

3. Committen Sie die Datei. Nach kurzer Zeit können Sie die unter ["Settings / Pages"](../../settings/pages) veröffentlichte URL der GitHub Page besuchen und sehen dort den Inhalt des Ordners public.
