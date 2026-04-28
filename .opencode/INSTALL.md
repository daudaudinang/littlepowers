# Cài đặt LittlePowers cho OpenCode

## Yêu cầu

- [OpenCode.ai](https://opencode.ai) đã cài đặt

## Cài đặt

Thêm littlepowers vào mảng `plugin` trong `opencode.json` (global hoặc project-level):

```json
{
  "plugin": ["littlepowers@git+https://github.com/daudaudinang/littlepowers.git"]
}
```

Restart OpenCode. Xong — plugin tự cài và đăng ký tất cả skills.

Xác nhận bằng cách hỏi: "Tell me about your superpowers"

## Di chuyển từ cài đặt symlink cũ

Nếu trước đó bạn cài superpowers bằng `git clone` và symlinks, xóa setup cũ:

```bash
# Xóa symlinks cũ
rm -f ~/.config/opencode/plugins/superpowers.js
rm -rf ~/.config/opencode/skills/superpowers

# Tùy chọn: xóa repo đã clone
rm -rf ~/.config/opencode/superpowers

# Xóa skills.paths khỏi opencode.json nếu bạn đã thêm cho superpowers
```

Sau đó follow các bước cài đặt ở trên.

## Sử dụng

Dùng `skill` tool native của OpenCode:

```
use skill tool to list skills
use skill tool to load littlepowers/brainstorming
```

## Cập nhật

LittlePowers tự động cập nhật khi bạn restart OpenCode.

Pin phiên bản cụ thể:

```json
{
  "plugin": ["littlepowers@git+https://github.com/daudaudinang/littlepowers.git#v0.1.0"]
}
```

## Xử lý lỗi

### Plugin không load

1. Kiểm tra logs: `opencode run --print-logs "hello" 2>&1 | grep -i littlepowers`
2. Xác nhận dòng plugin trong `opencode.json`
3. Đảm bảo bạn đang chạy phiên bản OpenCode mới

### Không tìm thấy skills

1. Dùng `skill` tool để list những gì đã discover
2. Kiểm tra plugin đã load chưa (xem ở trên)

### Ánh xạ tools

Khi skills reference các Claude Code tools:
- `TodoWrite` → `todowrite`
- `Task` with subagents → cú pháp `@mention`
- `Skill` tool → `skill` tool native của OpenCode
- File operations → native tools của bạn

## Trợ giúp

- Báo lỗi: https://github.com/daudaudinang/littlepowers/issues
- Tài liệu đầy đủ: https://github.com/daudaudinang/littlepowers/blob/main/docs/README.opencode.md
