# App-TKJ - Platform Terpadu TKJ & Mikrotik Academy

[![Go Version](https://img.shields.io/badge/go-1.21+-blue.svg)](https://golang.org)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

## 📖 Deskripsi

**App-TKJ** adalah platform terpadu untuk:
- Website resmi TKJ (Teknik Komputer dan Jaringan)
- Sistem pembelajaran (Mikrotik Academy)
- CMS untuk manajemen konten

Platform ini dibangun dengan **Golang** (net/http) dan **PostgreSQL**, dirancang untuk deployment di **Proxmox LXC** tanpa Docker.

---

## 🚀 Fitur Utama

### ✅ CMS (Content Management System)
- Manajemen artikel (create, edit, delete, publish/draft)
- Halaman statis (About, Profile, dll)
- Rich content editor (HTML)

### ✅ Mikrotik Academy
- Manajemen kursus (courses)
- Modul pembelajaran terstruktur
- Progress tracking

### ✅ User Management
- Role-based access (Admin & User)
- Session-based authentication
- Secure password hashing (bcrypt)

### ✅ UI/UX Modern
- Dashboard modern dengan sidebar + topbar
- Responsive design (mobile-first)
- Animasi smooth (CSS-based)
- Lightweight & fast

---

## 🏗️ Arsitektur

```
┌─────────────────┐     ┌─────────────────┐
│   Proxmox VE    │     │   Proxmox VE    │
│  ┌───────────┐  │     │  ┌───────────┐  │
│  │  LXC 203  │  │     │  │  LXC 201  │  │
│  │  App-TKJ  │◄─┼─────┼──┤ PostgreSQL│  │
│  │  (Go)     │  │     │  │           │  │
│  │  Port 8090│  │     │  │  Port 5432│  │
│  └───────────┘  │     │  └───────────┘  │
└─────────────────┘     └─────────────────┘
```

### Tech Stack
- **Backend**: Golang (net/http, pgx)
- **Database**: PostgreSQL
- **Frontend**: HTML Templates, Tailwind CSS, Vanilla JS
- **Deployment**: LXC Container (No Docker)

---

## 📦 Instalasi & Deployment

### Prerequisites
- Go 1.21+
- PostgreSQL 14+
- Git

### 1. Clone Repository

```bash
git clone https://github.com/your-username/app-tkj.git
cd app-tkj
```

### 2. Setup Database

```bash
psql -h <DB_HOST> -U <DB_USER> -d db_web_tkj -f schema.sql
```

### 3. Konfigurasi Environment

Buat file `.env` dari `.env.example`:

```bash
cp .env.example .env
```

Edit `.env`:

```env
PORT=8090
DATABASE_URL=postgres://user:password@192.168.88.201:5432/db_web_tkj
SESSION_KEY=random_secure_key_32_chars
```

### 4. Build & Run

#### Development
```bash
go run main.go
```

#### Production
```bash
go build -o app-tkj
./app-tkj
```

---

## 🔧 Deployment di LXC

### 1. Upload Binary ke LXC

```bash
scp app-tkj root@192.168.88.203:/opt/app-tkj/
```

### 2. Setup Systemd Service

Buat file `/etc/systemd/system/app-tkj.service`:

```ini
[Unit]
Description=App TKJ - Platform Terpadu TKJ
After=network.target

[Service]
Type=simple
WorkingDirectory=/opt/app-tkj
ExecStart=/opt/app-tkj/app-tkj
Restart=always
RestartSec=5
User=root
Environment="PORT=8090"
Environment="DATABASE_URL=postgres://user:pass@192.168.88.201:5432/db_web_tkj"
Environment="SESSION_KEY=your_secure_key"

[Install]
WantedBy=multi-user.target
```

### 3. Enable & Start Service

```bash
systemctl daemon-reload
systemctl enable app-tkj
systemctl start app-tkj
systemctl status app-tkj
```

### 4. Update Aplikasi

```bash
cd /opt/app-tkj
git pull
go build -o app-tkj
systemctl restart app-tkj
```

---

## 🌐 Routing

### Public Routes
| Method | Endpoint     | Description      |
|--------|-------------|------------------|
| GET    | `/`         | Homepage         |
| GET    | `/articles` | List articles    |
| GET    | `/courses`  | List courses     |
| GET/POST| `/login`   | Login page       |
| POST   | `/logout`   | Logout           |

### Admin Routes
| Method | Endpoint            | Description      |
|--------|--------------------|------------------|
| GET    | `/admin/dashboard` | Dashboard        |
| GET/POST| `/admin/articles` | Manage articles  |
| GET/POST| `/admin/pages`    | Manage pages     |
| GET/POST| `/admin/courses`  | Manage courses   |
| GET/POST| `/admin/modules`  | Manage modules   |
| GET/POST| `/admin/users`    | Manage users     |

---

## 🗄️ Database Schema

Tables:
- `users` - User accounts (admin & user)
- `articles` - CMS articles
- `pages` - Static pages
- `courses` - Courses (Mikrotik Academy)
- `modules` - Course modules

Lihat `schema.sql` untuk detail struktur database.

---

## 🔐 Security Features

- ✅ Password hashing dengan bcrypt
- ✅ Session-based authentication
- ✅ Secure cookies
- ✅ Middleware protection untuk admin routes
- ✅ Input validation
- ✅ Basic CSRF protection

---

## 🎨 UI Features

- ✨ Modern dashboard design
- 📱 Fully responsive (mobile-first)
- 🎭 Smooth CSS animations
- ⚡ Fast loading (server-side rendering)
- 🎯 Clean & lightweight

---

## 🧪 Health Check

```bash
curl http://localhost:8090/health
```

Response:
```json
{ "status": "ok" }
```

---

## 📁 Struktur Project

```
app-tkj/
├── main.go              # Entry point
├── config/              # Configuration
├── models/              # Database models
├── handlers/            # HTTP handlers
├── middleware/          # Auth & other middleware
├── templates/           # HTML templates
├── static/
│   ├── css/             # Stylesheets
│   └── js/              # JavaScript files
├── utils/               # Utility functions
├── schema.sql           # Database schema
├── .env.example         # Environment template
└── .gitignore
```

---

## 🛠️ Development Phases

1. **Phase 1**: Setup backend core, DB connection, routing
2. **Phase 2**: Authentication system
3. **Phase 3**: CMS (articles, pages)
4. **Phase 4**: Courses & modules
5. **Phase 5**: UI polish & animations

---

## 📝 License

MIT License - lihat file [LICENSE](LICENSE) untuk detail.

---

## 👥 Contributing

Contributions are welcome! Silakan buat issue atau pull request.

---

## 📞 Support

Untuk pertanyaan atau bantuan, silakan buat issue di repository ini.

---

**Built with ❤️ for TKJ Students & Teachers**
