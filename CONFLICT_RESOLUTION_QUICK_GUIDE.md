# ⚡ Quick Conflict Resolution Guide

## 🎯 When Conflict Happens

```
CONFLICT (content): Merge conflict in styles.css
CONFLICT (content): Merge conflict in script.js
Automatic merge failed; fix conflicts and then commit the result.
```

---

## 🔍 Step-by-Step Resolution

### Step 1: Check Status
```bash
git status
```
Output akan menunjukkan "both modified" files:
```
both modified:   styles.css
both modified:   script.js
```

### Step 2: View Conflict Markers
```bash
# Lihat file dengan conflict
cat styles.css

# Atau open di editor (VS Code, Sublime, etc)
code styles.css
```

Conflict markers terlihat seperti:
```
<<<<<<< HEAD
[content from current branch]
=======
[content from incoming branch]
>>>>>>> feature-redesign
```

### Step 3: Decide Resolution Strategy

**3 Pilihan:**

#### ✅ Option A: Accept Current (Dark Mode)
Gunakan styling dark mode, buang redesign styling:
```bash
git checkout --ours styles.css
git checkout --ours script.js
```

#### ✅ Option B: Accept Incoming (Redesign)
Gunakan redesign styling, buang dark mode:
```bash
git checkout --theirs styles.css
git checkout --theirs script.js
```

#### ✅ Option C: Manual Merge (RECOMMENDED)
Edit file manually dan combine keduanya.

---

## 🛠️ Manual Resolution (Recommended)

### File: styles.css

Cari conflict markers dan resolve:

```css
/* BEFORE (conflict) */
<<<<<<< HEAD
body {
    color: #fff;
    background-color: #1a1a1a;
}
=======
body {
    color: #333;
}
>>>>>>> feature-redesign

/* AFTER (resolved) */
body {
    color: #333;
    background-color: #fff;
}

body.dark-mode {
    color: #fff;
    background-color: #1a1a1a;
}
```

**Pattern untuk semua section:**
- Navbar colors → support both light & dark
- Button colors → support both light & dark
- Service cards → support both light & dark
- Tambahkan portfolio styling dari redesign
- Tambahkan dark mode toggle styles

**Hasil akhir yang ideal:**
```css
/* Light mode (default) */
body { color: #333; background: white; }
.navbar { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); }
.btn { background-color: #667eea; }

/* Dark mode */
body.dark-mode { color: #fff; background: #1a1a1a; }
body.dark-mode .navbar { background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%); }
body.dark-mode .btn { background-color: #00d4ff; }

/* Portfolio section (dari redesign) */
.portfolio { ... }
.portfolio-item { ... }
```

### File: script.js

Combine functions dari kedua branch:

```javascript
/* BEFORE (conflict) */
<<<<<<< HEAD
// Dark mode toggle function
function toggleDarkMode() {
    document.body.classList.toggle('dark-mode');
}
=======
// Portfolio filter
document.querySelectorAll('.portfolio-item').forEach(item => {
    item.addEventListener('mouseenter', function() {
        this.style.transform = 'scale(1.05)';
    });
});
>>>>>>> feature-redesign

/* AFTER (resolved) */
// Dark mode toggle function
function toggleDarkMode() {
    document.body.classList.toggle('dark-mode');
    localStorage.setItem('darkMode', document.body.classList.contains('dark-mode'));
}

// Portfolio interactions
document.querySelectorAll('.portfolio-item').forEach(item => {
    item.addEventListener('mouseenter', function() {
        this.style.transform = 'scale(1.05)';
    });
    item.addEventListener('mouseleave', function() {
        this.style.transform = 'scale(1)';
    });
});

// Apply saved preference
if (localStorage.getItem('darkMode') === 'true') {
    document.body.classList.add('dark-mode');
}
```

---

## ✅ After Manual Edit

### 1. Stage Resolved Files
```bash
git add styles.css
git add script.js
git add index.html  # jika ada conflict di sini
```

### 2. Commit Merge
```bash
git commit -m "Merge feature-redesign: Combine dark-mode + portfolio with responsive design"
```

### 3. Verify Merge Success
```bash
git log --oneline -5
# Output:
# abc1234 (HEAD -> test) Merge feature-redesign: Combine dark-mode + portfolio...
# 2ed1f19 (feature-redesign) feat: Add portfolio section
# d13d3a8 (feature-dark-mode) feat: Add dark mode theme
# def5678 Initial commit
```

### 4. Push to Remote
```bash
git push origin test
```

---

## 🐛 Troubleshooting

### Problem: "Merge conflict marked as resolved, but it was not"
**Solution:**
```bash
# Batalkan merge
git merge --abort

# Mulai ulang
git pull origin test
git merge feature-redesign
```

### Problem: "Cannot push - updates rejected"
**Solution:**
```bash
# Pull latest changes terlebih dahulu
git pull origin test

# Jika masih ada conflict, resolve lagi
# Lalu push
git push origin test
```

### Problem: "Accidentally resolved conflict wrong"
**Solution:**
```bash
# Lihat yang sudah di-stage
git diff --cached

# Unstage jika belum commit
git reset

# Atau undo last commit jika sudah commit
git reset HEAD~1
```

---

## 📋 Conflict Resolution Checklist

- [ ] Read conflict markers carefully
- [ ] Understand what changed in each branch
- [ ] Decide: keep current, keep incoming, or combine
- [ ] Edit file(s) manually
- [ ] Remove conflict markers (<<<, ===, >>>)
- [ ] Test edited code (buka di browser, test functionality)
- [ ] `git add` untuk semua resolved files
- [ ] `git commit` dengan meaningful message
- [ ] `git push` untuk update remote
- [ ] Verify PR shows "Resolved" status

---

## 🧪 Quick Test After Merge

```bash
# 1. Check git history
git log --graph --oneline --all

# 2. Test file content
cat styles.css | grep "dark-mode"  # Should exist
cat styles.css | grep "portfolio"  # Should exist

# 3. Open HTML file di browser
# - Check dark mode is applied
# - Check portfolio section exists
# - Check all styling looks good

# 4. Check no leftover conflict markers
grep -r "<<<<<<" .
grep -r "======" .
grep -r ">>>>>>" .
# Output should be empty if clean
```

---

## 💡 Common Conflict Scenarios

### Scenario 1: Both sides changed same variable
```
<<<<<<< HEAD
const primaryColor = '#1a1a2e';  // dark mode
=======
const primaryColor = '#667eea';  // light mode
>>>>>>> feature-redesign

// Resolve: Choose or create variable for both
const primaryColor = '#667eea';     // light
const darkPrimaryColor = '#1a1a2e'; // dark
```

### Scenario 2: One side added, other side modified
```
<<<<<<< HEAD
.navbar {
    background: #1a1a2e;
    color: white;
}
=======
.navbar {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 1rem 0;
    position: sticky;
}
>>>>>>> feature-redesign

// Resolve: Combine both features
.navbar {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 1rem 0;
    position: sticky;
}

.dark-mode .navbar {
    background: #1a1a2e;
}
```

### Scenario 3: Completely different approaches
```
// Analyze both approaches
// Choose the better one OR
// Refactor to support both

// This is where code review helps!
```

---

## 📞 When to Ask for Help

❌ **Don't:** Just delete entire sections to "fix" conflict
❌ **Don't:** Keep both versions without understanding
❌ **Don't:** Ignore conflict markers

✅ **Do:** Take time to understand changes
✅ **Do:** Test after resolution
✅ **Do:** Communicate with team
✅ **Do:** Use merge commit message clearly

---

## 🎓 Learning Resource

After resolving this conflict:
1. Take screenshot of conflict markers
2. Document how you resolved it
3. Write summary of lessons learned
4. Share dengan team untuk learning

**Example Summary:**
```
## Conflict Resolution Summary

### Root Cause
feature-dark-mode dan feature-redesign kedua-duanya 
mengubah styles.css untuk styling changes.

### Resolution Approach
Manual merge dengan combining both approaches:
- Kept dark mode colors dan light mode as default
- Kept portfolio section dari redesign
- Updated button colors untuk support both themes

### Key Learnings
1. Conflict terjadi karena edited same lines
2. Manual merge lebih baik daripada automatic
3. Support multiple themes dari awal lebih mudah

### Prevention
Untuk project selanjutnya:
- Separate theme-related styling di file terpisah
- Communicate tentang file changes di team
- Rebase frequently untuk avoid large merge conflicts
```

