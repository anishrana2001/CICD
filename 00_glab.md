### glab is the official open-source command-line interface (CLI) tool for GitLab. It brings GitLab's full functionality directly into your terminal, allowing you to work without constantly switching to the web browser.
### **Why Use glab?**
#### Traditional GitLab workflows force constant context switching—your code lives in the terminal with git, but GitLab issues, merge requests, pipelines, and releases require opening browser tabs. glab eliminates this by providing native terminal access to GitLab's REST API, right alongside your existing git workflow.

### Key Benefits
- **CI/CD Pipeline Management**: View pipeline status, retry failed jobs, and approve manual stages directly from CLI (glab ci view, glab ci retry).
- **Issue & MR Workflows**: Create, list, comment, label, and close issues/merge requests without browser tabs (glab issue list, glab mr create).
- **Self-Hosted Support**: Perfect for your Servera (10.10.10.16) GitLab instance—automatically detects hostname from git remotes.
- **Multi-Instance**: Switch between GitLab.com, self-hosted, or Dedicated instances seamlessly.
- **Changelogs & Releases**: Auto-generate changelogs and manage releases (glab release create).
- **No Context Switching**: Stay in your terminal (VS Code, iTerm) where you're already coding and running CI/CD locally.


## How to install it on Mac?
```
brew install glab
```

## Verify it!
```
glab --version
```
