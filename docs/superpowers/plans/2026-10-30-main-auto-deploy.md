# Main Branch Auto Deployment Plan

**Goal:** Make source changes pushed to `main` automatically publish the GitHub Pages mirror and Cloudflare Worker, while keeping daily collection and deployment working.

**Architecture:** Promote the current `master` history to `main` and make it the repository default. Update the existing scheduled backup workflow to build from `main`, and add push-triggered GitHub Pages and Cloudflare deployments without running data collection. Cloudflare deployment uses repository secrets and reports a clear skip until they are configured.

**Tech Stack:** GitHub Actions, Python `build.py`, Node `collect/pages_build.mjs`, GitHub Pages, Wrangler.

**Verification:** Build the Pages artifact locally, statically validate workflow triggers and branch refs, then inspect the GitHub Actions run started by the pushed `main` commit.

## Global Constraints

- Preserve daily backup, monitoring, and independent collection behavior.
- Avoid running collection as a side effect of ordinary source pushes.
- Use GitHub Pages' built-in `GITHUB_TOKEN` permissions for deployment.
- Do not overwrite or discard any existing remote branch history.

## Review Focus

- A push to `main` builds and publishes pages without invoking data collection.
- Scheduled/manual collection continues to deploy using the repository's default branch.
- Existing `/freedom8964.com` URL prefix and 3-language page generation remain intact.

---

### Task 1: Enable Main Branch Pages Deployment

**Files:**
- Modify: `.github/workflows/backup.yml`
- Create: `.github/workflows/cloudflare-deploy.yml`
- Modify: `README.md`

- [ ] Add `main` push trigger for site source and page data changes.
- [ ] Keep push-triggered deployment separate from collection and backup jobs.
- [ ] Change scheduled Pages checkout from `master` to `main`.
- [ ] Deploy the Cloudflare Worker on source pushes when both Cloudflare secrets are set.
- [ ] Document the two required Cloudflare secrets.
- [ ] Validate workflow syntax, trigger paths, and branch refs.
- [ ] Build the Pages artifact and verify representative routes exist.
- [ ] Push `main`, set it as the repository default, and inspect its deployment run.
