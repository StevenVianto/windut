# 🔄 Push & Pull Request Instructions

## 📤 Step 1: Push Branches ke Remote

```bash
# Push feature-dark-mode branch
git push origin feature-dark-mode

# Push feature-redesign branch
git push origin feature-redesign

# Push main branch
git push origin test
```

**Output yang diharapkan:**
```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 400 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:username/repo.git
 * [new branch]      feature-dark-mode -> feature-dark-mode
```

---

## 🔗 Step 2: Buat Pull Request dari GitHub/GitLab

### Untuk `feature-dark-mode`:

1. Pergi ke GitHub/GitLab repo
2. Klik "New Pull Request"
3. Set:
   - **Base branch:** `test` (main)
   - **Compare branch:** `feature-dark-mode`
4. Title: `feat: Add dark mode theme to website`
5. Description:
```
## Dark Mode Theme Implementation

### Changes
- Updated color scheme untuk dark theme
- Navbar: #667eea → #1a1a2e
- Services cards: white → #2a2a2a
- Buttons: updated dengan cyan color (#00d4ff)
- Added dark mode background untuk body

### Testing
- Tested di Chrome, Firefox, Safari
- Responsive design maintained
- Dark mode toggle ready untuk integration

### Screenshots
[Kalau ada screenshot dark mode]

Closes #123
```

### Untuk `feature-redesign`:

1. Buat PR baru dengan:
   - **Base branch:** `test`
   - **Compare branch:** `feature-redesign`
2. Title: `feat: Add portfolio section and improve website layout`
3. Description:
```
## Portfolio Section & Layout Redesign

### Changes
- ✨ Tambah portfolio section dengan 4 project showcase
- 📱 Updated navbar dengan link ke portfolio
- 🎨 Enhanced styling dengan gradient backgrounds
- ⚡ Added hover animations untuk portfolio items
- 🔧 Updated JavaScript dengan portfolio interactions

### New Files
- Portfolio section di index.html
- Portfolio styling di styles.css
- Portfolio animation di script.js

### Breaking Changes
- None

### Testing
- Semua section terload dengan benar
- Portfolio grid responsive di mobile
- Hover effects smooth dan user-friendly

Closes #456
```

---

## ⚠️ Expected Conflict Warning

Setelah membuat PR kedua, GitHub akan menunjukkan:

```
❌ Can't automatically merge
This branch has conflicts that can't be automatically merged.

Use the command line to resolve conflicts before continuing.
```

**INI ADALAH EXPECTED!** Ini adalah scenario yang kita inginkan untuk studi kasus.

---

## 🔧 Cara Handle Conflict di GitHub UI

### Option 1: Resolve di GitHub (Simple)
1. Klik "Resolve conflicts" button
2. Edit conflict markers secara manual
3. Klik "Mark as resolved"
4. Commit merge

### Option 2: Resolve Locally (Recommended)
```bash
# Fetch latest dari remote
git fetch origin

# Update feature-redesign dengan latest main
git checkout feature-redesign
git pull origin test

# Merge main ke feature-redesign locally
git merge test

# Sekarang akan muncul conflict - resolve manually
# Edit files dengan conflict
# git add .
# git commit -m "Resolve merge conflicts"

# Push hasil merge ke remote
git push origin feature-redesign

# PR akan auto update dan show as "Ready to merge" ✅
```

---

## ✅ Verify Branches Status

Cek semua branch dan commit mereka:

```bash
# Lihat semua branch dengan info commit terbaru
git branch -vv

# Output:
# feature-dark-mode     d13d3a8 [origin/feature-dark-mode] feat: Add dark mode theme
# feature-redesign      2ed1f19 [origin/feature-redesign] feat: Add portfolio section
# test                  [tracking origin/test] Initial commit
```

---

## 📊 Git Flow Visualization

```
test (main)
    ↓
    ├─→ feature-dark-mode (commit: d13d3a8)
    │   └─→ Changes: styles.css (colors)
    │
    └─→ feature-redesign (commit: 2ed1f19)
        └─→ Changes: index.html, styles.css, script.js
        
        ❌ CONFLICT akan terjadi di:
           - styles.css (styling changes)
           - script.js (function additions)
```

---

## 🎓 Learning Objectives

✅ Memahami bagaimana conflict terjadi
✅ Lihat conflict markers di real scenario
✅ Praktik resolve conflict manually
✅ Memahami merge strategy
✅ Familiar dengan Git commands
✅ Biasa dengan PR review process

---

## 💻 Complete Commands Reference

```bash
# Clone repo (jika belum punya)
git clone <repo-url>
cd windut

# Setup initial commit
git add .
git commit -m "Initial commit"

# Create branches
git branch feature-dark-mode
git branch feature-redesign

# Checkout dan work di branch
git checkout feature-dark-mode
# (make changes)
git add .
git commit -m "feat: Add dark mode"
git push origin feature-dark-mode

# Switch ke branch lain
git checkout feature-redesign
# (make changes)
git add .
git commit -m "feat: Add portfolio"
git push origin feature-redesign

# View all branches
git branch -a

# View branch differences
git diff test feature-dark-mode
git diff test feature-redesign

# Try merging locally first (untuk test)
git checkout test
git merge feature-dark-mode  # ✅ Success
git merge feature-redesign   # ❌ Conflict!

# Abort merge jika tidak siap
git merge --abort

# Check conflict status
git status

# View conflict files
git diff --name-only --diff-filter=U

# Setelah resolve conflict manual
git add .
git commit -m "Merge: Resolve conflicts between dark-mode dan redesign"
```

---

## 🚀 Next Steps

1. **Push semua branches**
2. **Create PR untuk feature-dark-mode** → should merge without conflict
3. **Create PR untuk feature-redesign** → will have conflicts
4. **Resolve conflicts** menggunakan salah satu method di atas
5. **Review dan merge** ke main branch
6. **Delete feature branches** setelah merge

```bash
# After merging
git branch -d feature-dark-mode
git branch -d feature-redesign
git push origin --delete feature-dark-mode
git push origin --delete feature-redesign
```

---

## 📌 Summary Table

| Branch | Conflicts | PR Status |
|--------|-----------|-----------|
| feature-dark-mode | ❌ None | ✅ Can merge |
| feature-redesign | ⚠️ Yes | ❌ Cannot merge (needs resolve) |
| After resolve | ✅ Fixed | ✅ Can merge |

