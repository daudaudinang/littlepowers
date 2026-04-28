# Cài đặt LittlePowers cho Codex

## Phương thức 1 — Plugin Marketplace (khuyến nghị)

Yêu cầu Codex CLI phiên bản hỗ trợ `plugin marketplace`:

```bash
codex plugin marketplace add daudaudinang/littlepowers
```

Restart Codex — plugin tự động được install và hiện trong danh sách plugin.

### Cách hoạt động

Lệnh `marketplace add` clone repo về và đọc `.agents/plugins/marketplace.json` — file catalog liệt kê plugin `littlepowers` với policy `INSTALLED_BY_DEFAULT`. Codex install plugin vào `~/.codex/plugins/cache/littlepowers/littlepowers/local/` và load skills từ đó.

### Cập nhật

```bash
codex plugin marketplace upgrade
```

### Gỡ cài đặt

```bash
codex plugin marketplace remove daudaudinang/littlepowers
```

---

## Phương thức 2 — Git Clone + Symlink (fallback)

Dùng khi Codex CLI chưa hỗ trợ `plugin marketplace`, hoặc bạn muốn kiểm soát thủ công.

### Yêu cầu

- Git

### Cài đặt

1. **Clone repository littlepowers:**
   ```bash
   git clone https://github.com/daudaudinang/littlepowers.git ~/.codex/littlepowers
   ```

2. **Tạo symlink cho skills:**
   ```bash
   mkdir -p ~/.agents/skills
   ln -s ~/.codex/littlepowers/skills ~/.agents/skills/littlepowers
   ```

   **Windows (PowerShell):**
   ```powershell
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
   cmd /c mklink /J "$env:USERPROFILE\.agents\skills\littlepowers" "$env:USERPROFILE\.codex\littlepowers\skills"
   ```

3. **Restart Codex** (thoát và mở lại CLI) để discover skills.

### Xác nhận

```bash
ls -la ~/.agents/skills/littlepowers
```

### Cập nhật

```bash
cd ~/.codex/littlepowers && git pull
```

Skills cập nhật ngay qua symlink.

### Gỡ cài đặt

```bash
rm ~/.agents/skills/littlepowers
# Tùy chọn xóa clone:
rm -rf ~/.codex/littlepowers
```

---

## Di chuyển từ bootstrap cũ

Nếu bạn cài superpowers trước khi có native skill discovery:

1. **Cập nhật repo:**
   ```bash
   cd ~/.codex/littlepowers && git pull
   ```

2. **Tạo symlink cho skills** (Phương thức 2, bước 2 ở trên) — đây là cơ chế discovery mới.

3. **Xóa block bootstrap cũ** khỏi `~/.codex/AGENTS.md` — block nào reference `superpowers-codex bootstrap` không còn cần.

4. **Restart Codex.**
