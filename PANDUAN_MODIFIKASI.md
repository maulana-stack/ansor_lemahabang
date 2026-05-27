# 📘 Panduan Modifikasi Simpel & Jelas

## Daftar Isi
1. [Memodifikasi Warna & Tema](#memodifikasi-warna--tema)
2. [Menambah Field Baru](#menambah-field-baru)
3. [Menambah Halaman Baru](#menambah-halaman-baru)
4. [Menambah API Baru](#menambah-api-baru)
5. [Mengubah Teks & Label](#mengubah-teks--label)
6. [Menambah Fitur Baru](#menambah-fitur-baru)

---

## 1️⃣ Memodifikasi Warna & Tema

### Langkah 1: Buka file Tailwind Config
📁 **File**: `frontend/tailwind.config.js`

**Sebelum:**
```js
theme: {
  extend: {
    colors: {
      primary: '#3B82F6',      // Biru
      secondary: '#10B981',    // Hijau
      danger: '#EF4444',       // Merah
      warning: '#F59E0B',      // Kuning
    },
  },
},
```

**Sesudah (Contoh: Ubah Biru jadi Ungu):**
```js
theme: {
  extend: {
    colors: {
      primary: '#8B5CF6',      // Ungu (ubah dari biru)
      secondary: '#10B981',    // Hijau
      danger: '#EF4444',       // Merah
      warning: '#F59E0B',      // Kuning
    },
  },
},
```

**Warna yang bisa dipakai:**
- `#FF6B6B` - Merah muda
- `#4ECDC4` - Cyan
- `#45B7D1` - Biru muda
- `#96CEB4` - Hijau muda
- `#FFEAA7` - Kuning muda

---

## 2️⃣ Menambah Field Baru

### Contoh: Tambah "Nomor Telepon" di User

#### Step 1: Edit Backend Model
📁 **File**: `backend/models/User.js`

**Temukan section fields, tambahkan:**
```js
bio: {
  type: String,
  default: '',
},
// ➕ TAMBAHKAN INI:
phone: {
  type: String,
  default: '',
},
```

#### Step 2: Update Frontend Form
📁 **File**: `frontend/src/pages/Profile.jsx`

**Temukan form fields, tambahkan:**
```jsx
<div>
  <label className="block text-sm font-medium text-gray-700">Bio</label>
  <textarea
    value={formData.bio}
    onChange={(e) => setFormData({ ...formData, bio: e.target.value })}
    disabled={!isEditing}
    rows="4"
    className="mt-1 block w-full border border-gray-300 rounded-lg px-3 py-2 disabled:bg-gray-50 disabled:text-gray-600"
  />
</div>

{/* ➕ TAMBAHKAN INI: */}
<div>
  <label className="block text-sm font-medium text-gray-700">Nomor Telepon</label>
  <input
    type="tel"
    value={formData.phone}
    onChange={(e) => setFormData({ ...formData, phone: e.target.value })}
    disabled={!isEditing}
    className="mt-1 block w-full border border-gray-300 rounded-lg px-3 py-2 disabled:bg-gray-50 disabled:text-gray-600"
    placeholder="+62812345678"
  />
</div>
```

#### Step 3: Update Context API
📁 **File**: `frontend/src/context/AuthContext.jsx`

**Di `register` function, tambahkan phone:**
```js
const register = async (name, email, password, phone) => {
  try {
    setLoading(true)
    const response = await api.post('/auth/register', { 
      name, 
      email, 
      password,
      phone  // ➕ Tambahkan ini
    })
    // ... rest code
```

---

## 3️⃣ Menambah Halaman Baru

### Contoh: Menambah "Settings" Page

#### Step 1: Buat File Settings Page
📁 **Buat file baru**: `frontend/src/pages/Settings.jsx`

```jsx
import { useState } from 'react'
import Navbar from '../components/Navbar'

export default function Settings() {
  const [settings, setSettings] = useState({
    notifications: true,
    darkMode: false,
    language: 'id'
  })

  const handleChange = (field, value) => {
    setSettings({ ...settings, [field]: value })
  }

  const handleSave = () => {
    console.log('Settings saved:', settings)
    alert('Pengaturan berhasil disimpan!')
  }

  return (
    <>
      <Navbar />
      <main className="min-h-screen bg-gray-50">
        <div className="max-w-2xl mx-auto py-12 px-4">
          <div className="bg-white rounded-lg shadow p-6">
            <h1 className="text-2xl font-bold mb-6">Pengaturan</h1>

            {/* Notifikasi */}
            <div className="space-y-4 pb-6 border-b">
              <label className="flex items-center">
                <input
                  type="checkbox"
                  checked={settings.notifications}
                  onChange={(e) => handleChange('notifications', e.target.checked)}
                  className="w-4 h-4"
                />
                <span className="ml-3 text-gray-700">Aktifkan Notifikasi</span>
              </label>
            </div>

            {/* Dark Mode */}
            <div className="space-y-4 py-6 border-b">
              <label className="flex items-center">
                <input
                  type="checkbox"
                  checked={settings.darkMode}
                  onChange={(e) => handleChange('darkMode', e.target.checked)}
                  className="w-4 h-4"
                />
                <span className="ml-3 text-gray-700">Mode Gelap</span>
              </label>
            </div>

            {/* Bahasa */}
            <div className="space-y-4 py-6">
              <label className="block text-sm font-medium text-gray-700">Bahasa</label>
              <select
                value={settings.language}
                onChange={(e) => handleChange('language', e.target.value)}
                className="w-full border border-gray-300 rounded-lg px-3 py-2"
              >
                <option value="id">Indonesia</option>
                <option value="en">English</option>
              </select>
            </div>

            {/* Save Button */}
            <div className="flex justify-end mt-6">
              <button
                onClick={handleSave}
                className="bg-primary text-white px-6 py-2 rounded-lg hover:bg-blue-700"
              >
                Simpan Pengaturan
              </button>
            </div>
          </div>
        </div>
      </main>
    </>
  )
}
```

#### Step 2: Tambah Route di App.jsx
📁 **File**: `frontend/src/App.jsx`

```jsx
import Settings from './pages/Settings'  // ➕ Import di atas

// Dalam Routes, tambahkan:
<Route path="/settings" element={<PrivateRoute><Settings /></PrivateRoute>} />
```

#### Step 3: Tambah Link di Navbar
📁 **File**: `frontend/src/components/Navbar.jsx`

```jsx
<div className="hidden md:flex items-center space-x-6">
  <Link to="/" className="text-gray-700 hover:text-primary">Dashboard</Link>
  <Link to="/projects" className="text-gray-700 hover:text-primary">Projects</Link>
  {/* ➕ TAMBAHKAN INI: */}
  <Link to="/settings" className="text-gray-700 hover:text-primary">Settings</Link>
  {user?.role === 'admin' && (
    <Link to="/admin" className="text-gray-700 hover:text-primary">Admin</Link>
  )}
</div>
```

---

## 4️⃣ Menambah API Baru

### Contoh: API untuk Get Statistics

#### Step 1: Buat Route Baru
📁 **Buat file baru**: `backend/routes/stats.js`

```js
import express from 'express'
import Project from '../models/Project.js'
import Task from '../models/Task.js'
import { protect } from '../middleware/auth.js'

const router = express.Router()

// @route   GET /api/v1/stats/dashboard
// @desc    Get dashboard statistics
// @access  Private
router.get('/dashboard', protect, async (req, res) => {
  try {
    const projects = await Project.countDocuments({ owner: req.user.id })
    const tasks = await Task.countDocuments({ createdBy: req.user.id })
    const completedTasks = await Task.countDocuments({
      createdBy: req.user.id,
      status: 'completed'
    })

    res.status(200).json({
      success: true,
      data: {
        totalProjects: projects,
        totalTasks: tasks,
        completedTasks: completedTasks,
        completionRate: tasks > 0 ? Math.round((completedTasks / tasks) * 100) : 0
      }
    })
  } catch (error) {
    res.status(500).json({
      success: false,
      message: error.message
    })
  }
})

export default router
```

#### Step 2: Register Route di Server
📁 **File**: `backend/server.js`

```js
import statsRoutes from './routes/stats.js'  // ➕ Import di atas

// Dalam routes section, tambahkan:
app.use(`${basePath}/stats`, statsRoutes)
```

#### Step 3: Gunakan di Frontend
📁 **File**: `frontend/src/pages/Dashboard.jsx`

```jsx
import { useEffect, useState } from 'react'
import api from '../services/api'

export default function Dashboard() {
  const [stats, setStats] = useState(null)

  useEffect(() => {
    const fetchStats = async () => {
      try {
        const response = await api.get('/stats/dashboard')
        setStats(response.data.data)
      } catch (error) {
        console.error('Failed to fetch stats:', error)
      }
    }
    fetchStats()
  }, [])

  if (!stats) return <div>Loading...</div>

  return (
    <>
      {/* Display stats */}
      <div className="bg-white p-6 rounded-lg">
        <p>Total Projects: {stats.totalProjects}</p>
        <p>Total Tasks: {stats.totalTasks}</p>
        <p>Completion Rate: {stats.completionRate}%</p>
      </div>
    </>
  )
}
```

---

## 5️⃣ Mengubah Teks & Label

### Langkah Simpel:

#### Ubah Nama Aplikasi
📁 **File**: `frontend/src/components/Navbar.jsx`

**Temukan:**
```jsx
<Link to="/" className="flex items-center">
  <span className="text-2xl font-bold text-primary">Ansor</span>
</Link>
```

**Ubah menjadi:**
```jsx
<Link to="/" className="flex items-center">
  <span className="text-2xl font-bold text-primary">My App</span>
</Link>
```

#### Ubah Judul Dashboard
📁 **File**: `frontend/src/pages/Dashboard.jsx`

**Temukan:**
```jsx
<h1 className="text-3xl font-bold text-gray-900">
  Welcome back, {user?.name}! 👋
</h1>
```

**Ubah menjadi:**
```jsx
<h1 className="text-3xl font-bold text-gray-900">
  Halo, {user?.name}! 👋
</h1>
```

---

## 6️⃣ Menambah Fitur Baru

### Contoh: Tambah "Export ke PDF"

#### Step 1: Install Library
```bash
cd frontend
npm install html2pdf.js
```

#### Step 2: Buat Export Function
📁 **Buat file baru**: `frontend/src/services/export.js`

```js
import html2pdf from 'html2pdf.js'

export const exportProjectsToPDF = (projects) => {
  const element = document.createElement('div')
  element.innerHTML = `
    <h1>Laporan Proyek</h1>
    <p>Tanggal: ${new Date().toLocaleDateString('id-ID')}</p>
    <table border="1" cellpadding="10">
      <tr>
        <th>Nama Proyek</th>
        <th>Status</th>
        <th>Members</th>
      </tr>
      ${projects.map(p => `
        <tr>
          <td>${p.name}</td>
          <td>${p.status}</td>
          <td>${p.members.length}</td>
        </tr>
      `).join('')}
    </table>
  `

  const opt = {
    margin: 10,
    filename: 'laporan_proyek.pdf',
    image: { type: 'jpeg', quality: 0.98 },
    html2canvas: { scale: 2 },
    jsPDF: { orientation: 'portrait', unit: 'mm', format: 'a4' }
  }

  html2pdf().set(opt).from(element).save()
}
```

#### Step 3: Tambah Button di Projects Page
📁 **File**: `frontend/src/pages/Projects.jsx`

```jsx
import { exportProjectsToPDF } from '../services/export'

// Dalam Projects component, tambahkan button:
<button
  onClick={() => exportProjectsToPDF(projects)}
  className="flex items-center space-x-2 bg-green-600 text-white px-4 py-2 rounded-lg hover:bg-green-700"
>
  <span>📥 Export PDF</span>
</button>
```

---

## 🎯 Checklist Modifikasi

- [ ] Ubah warna tema di `tailwind.config.js`
- [ ] Update nama aplikasi di Navbar
- [ ] Tambah field baru di database model
- [ ] Update form di frontend
- [ ] Buat halaman baru jika diperlukan
- [ ] Tambah route di API jika diperlukan
- [ ] Register route baru di `server.js`
- [ ] Test semua perubahan

---

## 🐛 Troubleshooting

### Error: "Cannot find module"
```bash
# Solusi: Install dependencies lagi
npm install
```

### Error: "Port already in use"
```bash
# Kill process di port 5000 (backend)
lsof -ti:5000 | xargs kill -9

# Kill process di port 3000 (frontend)
lsof -ti:3000 | xargs kill -9
```

### React tidak reload
- Clear browser cache (Ctrl+F5)
- Restart development server

---

## 💡 Tips & Tricks

1. **Selalu commit setelah modifikasi**
```bash
git add .
git commit -m "Ubah warna tema menjadi ungu"
git push origin main
```

2. **Test di browser sebelum push**
- Login dengan akun test
- Coba semua fitur baru
- Check console untuk errors

3. **Gunakan Chrome DevTools**
- F12 → Console untuk debug
- Network tab untuk check API

4. **Dokumentasikan perubahan**
- Tulis di README apa yang diubah
- Biar mudah untuk kontributor lain

---

## 📞 Bantuan Cepat

Butuh bantuan? Template untuk error:

**Error Log:**
```
Di mana: backend/routes/projects.js line 25
Error: Cannot read property 'name' of undefined
Kapan: Saat create project
```

Dengan info ini, saya bisa membantu lebih cepat! 😊
