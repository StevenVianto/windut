# 📚 Studi Kasus: Git Merge Conflict

## 📋 Deskripsi Scenario

Ini adalah project website sederhana dengan 2 branch yang akan cause merge conflict. Project ini dibuat untuk pembelajaran tentang:
- Bagaimana conflict terjadi
- Cara mendeteksi conflict
- Cara menyelesaikan conflict
- Best practices dalam Git workflow

---

## 🌳 Struktur Branch

```
main (test)
├── feature-dark-mode
│   └── Perubahan: Styling dark theme
│       - Mengubah colors di styles.css
│       - Navbar background: purple → dark
│       - Services colors: #667eea → #00d4ff
│       - Button colors dan backgrounds
│
└── feature-redesign
    └── Perubahan: Redesign & portfolio section
        - Menambah section portofolio di index.html
        - Update styles.css dengan portfolio styling
        - Update script.js dengan portfolio animation
        - Menambah menu "Portofolio" di navbar
```

---

## ⚡ Mengapa Conflict Terjadi?

**File yang akan CONFLICT:**
1. **styles.css** - Kedua branch mengubah styling section (about, services, contact, buttons)
2. **index.html** - Feature redesign menambah portfolio, feature dark mode tidak touch ini
3. **script.js** - Feature redesign menambah portfolio animation

**Baris yang CONFLICT di styles.css:**
- Line 1-7: `body` styling (dark mode vs light mode)
- Line 12-19: `.navbar` colors (dark mode vs light mode)
- Line 29-39: `.btn` colors (dark mode vs light mode)
- Line 39-50: `.services` section styling
- Line 52-70: `.service-card` styling

---

## 🔧 Cara Mereproduksi Conflict

### Step 1: Lihat Semua Branch
```bash
git branch -a
```
Output:
```
  feature-dark-mode
* feature-redesign
  test
```

### Step 2: Cek Status Saat Ini
```bash
git log --oneline -5
```

### Step 3: Merge Feature Dark Mode ke Main
```bash
git checkout test
git merge feature-dark-mode
```
✅ **Hasil: SUCCESS** (tidak ada conflict, fast-forward merge)

### Step 4: Coba Merge Feature Redesign
```bash
git merge feature-redesign
```
❌ **Hasil: CONFLICT!** akan terlihat:
```
CONFLICT (content): Merge conflict in styles.css
CONFLICT (content): Merge conflict in script.js
Automatic merge failed; fix conflicts and then commit the result.
```

---

## 🔴 Conflict Markers

Saat conflict terjadi, file akan memiliki markers khusus:

```css
<<<<<<< HEAD (current branch)
/* Dark mode styling */
.navbar {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
}
=======
/* Light mode styling */
.navbar {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
>>>>>>> feature-redesign
```

**Penjelasan markers:**
- `<<<<<<< HEAD` = Perubahan dari branch saat ini (yang di-merge ke)
- `=======` = Separator
- `>>>>>>> branch-name` = Perubahan dari branch yang di-merge

---

## ✅ Cara Menyelesaikan Conflict

### Opsi 1: Manual Merge (Rekomendasi untuk Pembelajaran)

1. **Buka file yang conflict di editor:**
   - `styles.css`
   - `script.js`

2. **Untuk setiap conflict section, pilih:**
   - **Keep current** (dark mode) 
   - **Keep incoming** (redesign)
   - **Keep both** (gabungkan keduanya) ← RECOMMENDED!

3. **Contoh resolusi untuk styles.css (Keep Both):**
```css
/* Dark mode support */
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    line-height: 1.6;
    color: #fff;
    background-color: #1a1a1a;
    transition: background-color 0.3s, color 0.3s;
}

body.light-mode {
    color: #333;
    background-color: #fff;
}

/* Navbar - Support both themes */
.navbar {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
    color: white;
    padding: 1rem 0;
    position: sticky;
    top: 0;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}

body.light-mode .navbar {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}
```

4. **Hapus conflict markers** (<<<, ===, >>>)

5. **Stage file yang sudah resolved:**
```bash
git add styles.css
git add script.js
git add index.html
```

6. **Complete merge dengan commit:**
```bash
git commit -m "Merge feature-redesign with dark-mode support"
```

---

### Opsi 2: GUI Tool (Easier)

Gunakan VS Code atau tool lain:
```bash
git config merge.tool vscode
git mergetool
```

---

### Opsi 3: Abort Merge (Jika Salah)

Jika ingin batalkan merge:
```bash
git merge --abort
```

---

## 📝 Best Practices untuk Menghindari Conflict

1. **Bekerja di branch yang berbeda**
   ```bash
   git checkout -b feature/nama-fitur
   ```

2. **Sering pull dari main branch**
   ```bash
   git pull origin main
   ```

3. **Keep commits kecil dan fokus**
   - Jangan ubah 50 file sekaligus
   - 1 feature = 1 branch

4. **Komunikasi dalam tim**
   - Koordinasi siapa yang edit file mana
   - Code review sebelum merge

5. **Rebase sebelum merge**
   ```bash
   git rebase main
   # atau
   git pull --rebase origin main
   ```

---

## 🎯 Testing Setelah Merge

Setelah conflict resolved, pastikan:

1. **Test aplikasi web:**
   ```bash
   # Buka index.html di browser
   # Cek semua section visible
   # Cek dark mode toggle (jika ditambahkan)
   # Cek portfolio section render dengan benar
   ```

2. **Verify git history:**
   ```bash
   git log --oneline --graph --all
   ```
   Expected output:
   ```
   *   (HEAD -> main) Merge feature-redesign with dark-mode support
   |\
   | * (feature-redesign) Add portfolio section and redesign
   | * Initial commit
   |/
   * (feature-dark-mode) Add dark mode theme
   * Initial commit
   ```

---

## 🚀 Challenge Task

Setelah memahami concept ini, coba:

1. **Buat branch ke-3:** `feature-animations`
   - Ubah CSS dengan animations
   - Ubah script.js dengan bounce effects
   
2. **Merge ke main:**
   - Harusnya conflict dengan feature-redesign
   - Resolve manual di editor

3. **Merge feature-dark-mode:**
   - Gabungkan semua fitur jadi satu

---

## 📚 Resources

- [Official Git Documentation - Merge](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
- [GitHub - Resolving Merge Conflicts](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts)
- [Atlassian Git Tutorial - Merge Conflict](https://www.atlassian.com/git/tutorials/using-branches/merge-conflicts)

---

## 💡 Summary

| Aspek | Detail |
|-------|--------|
| **Tujuan** | Pembelajaran merge conflict handling |
| **Branch Count** | 2 feature branches + 1 main |
| **Conflict Files** | styles.css, script.js, index.html |
| **Severity** | Medium (manageable conflicts) |
| **Time to Resolve** | ~10-15 minutes manual |
| **Learning Value** | ⭐⭐⭐⭐⭐ |

