# Testing for Breaking Changes in Your Jekyll Portfolio

This guide explains how to safely test updates to your Ruby gems in the worktree environment.

## Why This Worktree is Perfect for Testing

You're currently in a git worktree:
- **Worktree path:** `/Users/yieldovercome/.claude-worktrees/julesconnectsdots/inspiring-edison`
- **Main repo:** `/Users/yieldovercome/Documents/_dev-portfolio/project_portfolio/julesconnectsdots`

The worktree is isolated, so you can experiment without affecting your main repository.

## Step-by-Step Testing Process

### 1. Backup Your Current Configuration

Before making any changes, save your current working setup:

```bash
cd /Users/yieldovercome/.claude-worktrees/julesconnectsdots/inspiring-edison
cp Gemfile.lock Gemfile.lock.backup
```

### 2. Try the Update

To update all gems to their latest compatible versions:

```bash
bundle update
```

Or to update a specific gem:

```bash
bundle update jekyll-sitemap
```

### 3. Test Your Site

Start the Jekyll server:

```bash
bundle exec jekyll serve
```

### 4. Check for Issues

Open `http://localhost:4000` in your browser and verify:

- [ ] Site builds without errors in Terminal
- [ ] Homepage loads correctly
- [ ] All pages render properly
- [ ] Blog posts display correctly
- [ ] Projects section works
- [ ] Navigation functions
- [ ] Styles look correct
- [ ] Images load

### 5. Decision Time

#### If Everything Works:

The updates are safe! Commit the updated `Gemfile.lock`:

```bash
git add Gemfile.lock
git commit -m "Update Ruby gems"
```

Then merge your worktree branch into main when ready.

#### If Something Breaks:

Restore your backup and investigate:

```bash
# Stop the Jekyll server (Ctrl+C)

# Restore the working version
mv Gemfile.lock.backup Gemfile.lock

# Reinstall the old versions
bundle install

# Verify it works again
bundle exec jekyll serve
```

### 6. Investigate the Problem (Optional)

If you want to find which gem caused the issue:

```bash
# See what changed
git diff Gemfile.lock.backup Gemfile.lock

# Try updating gems one at a time
bundle update jekyll-sitemap
bundle exec jekyll serve
# Test...

bundle update jekyll-toc
bundle exec jekyll serve
# Test...
```

## Common Breaking Changes to Watch For

- **Build errors** - Gem incompatibilities, deprecated features
- **Missing pages** - Routing or permalink changes
- **Broken styles** - Sass/CSS processing changes
- **CloudCannon issues** - CMS integration problems

## Notes

- Don't run `gem update --system` on old system Ruby (2.6.10)
- The worktree keeps your experiments separate from production
- `Gemfile.lock` is what prevents breaking changes in the first place
- Only update gems when you have a specific reason (security, new features, etc.)

## Your Current Setup (Working)

- Ruby: 2.6.10 (macOS system Ruby)
- Bundler: 2.3.26
- Jekyll: 4.3.3
- Gems locked as of: January 2024

This configuration is stable and working. Only update if necessary.
