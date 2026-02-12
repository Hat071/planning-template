# Branch Management

## Branch Registry

**Read `.planning/BRANCHES.md` before any implementation phase.** It tracks active branches, base branches, worktree paths, and project rules across all repos.

## Rules

- **Check before coding**: Know which repo + branch + base you're working in
- **Plan branches per phase**: Each implementation phase in `task_plan.md` should note which branches it touches
- **Update on change**: Create, merge, or abandon a branch → update BRANCHES.md in the same session
- **Log in progress.md**: Note branch operations (checkout, push, merge) with repo + commit hash
- **Worktree column**: Populate when using git worktrees; leave empty when using standard checkout

## Platform Detection

**Before running merge/PR commands, detect the platform from the git remote:**

```bash
git remote get-url origin 2>/dev/null
```

| Remote contains | Platform | CLI | Term |
|----------------|----------|-----|------|
| `github.com` | GitHub | `gh` | PR (Pull Request) |
| `gitlab` | GitLab | `glab` | MR (Merge Request) |

The **Platform** column in BRANCHES.md Project Rules table tracks this per repo. When adding a new repo, detect it once and record it.

## Merging — Always Use MR/PR

**All repos have protected default branches. Never push directly to main/master.**

### GitLab (`glab`)

1. **Create feature branch** and push: `git push -u origin <branch>`
2. **Create MR**: `glab mr create --squash-before-merge --remove-source-branch`
3. **Wait for CI**: `glab ci status`
4. **Merge**: `glab mr merge <number> --squash --remove-source-branch --yes`
5. **Clean up local**: `git checkout <default> && git pull && git branch -d <branch>`

### GitHub (`gh`)

1. **Create feature branch** and push: `git push -u origin <branch>`
2. **Create PR**: `gh pr create`
3. **Wait for CI**: `gh pr checks <number>`
4. **Merge**: `gh pr merge <number> --squash --delete-branch`
5. **Clean up local**: `git checkout <default> && git pull && git branch -d <branch>`

### Per-Project Squash Flags (GitLab)

Check the Project Rules table in BRANCHES.md before merging:

| Squash setting | `glab mr create` flag | `glab mr merge` flag |
|---------------|----------------------|---------------------|
| off (optional) | `--squash-before-merge` | `--squash` |
| default on | (auto) | `--squash` |
| always (required) | (auto) | (auto) |

### Approval Requirements

Some repos require specific approvals (tracked in Project Rules table). If "Approvals" is not "none", ensure the reviewer has approved the MR/PR before merging.

## Populating Project Rules

If the Project Rules table in BRANCHES.md has `?` entries or a new repo is added:

### Detect platform first

```bash
remote=$(git remote get-url origin 2>/dev/null)
if echo "$remote" | grep -q "github.com"; then
    echo "Platform: GitHub (gh)"
elif echo "$remote" | grep -qi "gitlab"; then
    echo "Platform: GitLab (glab)"
fi
```

### GitLab repos — query settings

```bash
cd <repo_path>

# Get merge settings
glab api projects/:id | python3 -c "
import sys,json; p=json.load(sys.stdin)
print(f'default_branch: {p[\"default_branch\"]}')
print(f'merge_method: {p[\"merge_method\"]}')
print(f'squash_option: {p[\"squash_option\"]}')
"

# Get branch protection
glab api projects/:id/protected_branches | python3 -c "
import sys,json
for b in json.load(sys.stdin):
    push = b['push_access_levels'][0]['access_level_description'] if b['push_access_levels'] else '?'
    merge = b['merge_access_levels'][0]['access_level_description'] if b['merge_access_levels'] else '?'
    print(f'{b[\"name\"]}: push={push}, merge={merge}, force_push={b[\"allow_force_push\"]}')
"
```

### GitHub repos — query settings

```bash
cd <repo_path>

# Get repo settings
gh api repos/{owner}/{repo} --jq '{default_branch, delete_branch_on_merge, allow_squash_merge, allow_merge_commit, allow_rebase_merge}'

# Get branch protection
gh api repos/{owner}/{repo}/branches/main/protection --jq '{required_pull_request_reviews: .required_pull_request_reviews.required_approving_review_count, enforce_admins: .enforce_admins.enabled}'
```

Update BRANCHES.md with the results.
