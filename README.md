# LittlePowers

LittlePowers là một phương pháp phát triển phần mềm toàn diện cho coding agent, được xây dựng trên nền tảng [Superpowers](https://github.com/obra/superpowers) — bộ skills composable và các hướng dẫn khởi tạo giúp agent tự động sử dụng chúng.

> **Fork từ**: [obra/superpowers](https://github.com/obra/superpowers) v5.0.7 — Cảm ơn [Jesse Vincent](https://blog.fsck.com) và team [Prime Radiant](https://primeradiant.com).

## Cách hoạt động

Mọi thứ bắt đầu từ khoảnh khắc bạn mở coding agent. Ngay khi nhận ra bạn muốn build gì đó, agent **không** nhảy thẳng vào viết code. Thay vào đó, nó lùi lại và hỏi bạn thực sự muốn làm gì.

Khi đã khai thác đủ yêu cầu từ cuộc trò chuyện, agent trình bày spec theo từng phần nhỏ — đủ ngắn để bạn thực sự đọc và hiểu.

Sau khi bạn duyệt thiết kế, agent soạn implementation plan rõ ràng đến mức một junior engineer nhiệt huyết (nhưng thiếu taste, thiếu judgement, không có project context, và ngại viết test) cũng có thể follow được. Plan nhấn mạnh true red/green TDD, YAGNI (You Aren't Gonna Need It), và DRY.

Tiếp theo, khi bạn nói "go", agent khởi động quy trình *subagent-driven-development* — các agent con xử lý từng engineering task, review lẫn nhau, rồi tiếp tục tiến lên. Không hiếm khi Claude tự chạy liên tục vài giờ mà không lệch khỏi plan bạn đã duyệt.

Còn nhiều thứ khác nữa, nhưng đó là cốt lõi của hệ thống. Và vì các skills tự động trigger, bạn không cần làm gì đặc biệt. Coding agent của bạn đơn giản là có Superpowers.

## Cài đặt

**Lưu ý:** Cách cài đặt khác nhau tùy platform. Chọn đúng platform bạn đang dùng.

### Claude Code

Đăng ký marketplace và cài plugin:

```bash
/plugin marketplace add daudaudinang/littlepowers
/plugin install littlepowers@littlepowers
```

Cập nhật: `/plugin marketplace update`

### OpenAI Codex CLI

Cài qua git và symlink:

```bash
git clone https://github.com/daudaudinang/littlepowers.git ~/.codex/littlepowers
mkdir -p ~/.agents/skills
ln -s ~/.codex/littlepowers/skills ~/.agents/skills/littlepowers
```

Restart Codex để discover skills.

**Chi tiết hơn:** [.codex/INSTALL.md](.codex/INSTALL.md)

### OpenAI Codex App

> ⚠️ Official plugin directory của Codex App hiện chưa mở cho third-party. Dùng Codex CLI ở trên.

### Cursor

Trong Cursor Agent chat:

```text
/add-plugin littlepowers
```

hoặc tìm "littlepowers" trong plugin marketplace.

### OpenCode

Thêm vào `opencode.json`:

```json
{
  "plugin": ["littlepowers@git+https://github.com/daudaudinang/littlepowers.git"]
}
```

Restart OpenCode. Plugin tự cài và đăng ký skills.

**Chi tiết hơn:** [.opencode/INSTALL.md](.opencode/INSTALL.md)

### GitHub Copilot CLI

```bash
copilot plugin marketplace add daudaudinang/littlepowers
copilot plugin install littlepowers@littlepowers
```

### Gemini CLI

```bash
gemini extensions install https://github.com/daudaudinang/littlepowers
```

Cập nhật:

```bash
gemini extensions update littlepowers
```

### Cài thủ công (mọi platform)

Nếu platform của bạn không hỗ trợ plugin marketplace, copy skills trực tiếp:

```bash
git clone https://github.com/daudaudinang/littlepowers.git
cp -r littlepowers/skills/ /path/to/your/project/.your-agent/skills/littlepowers/
```

Cập nhật: `cd littlepowers && git pull` rồi copy lại.

## Workflow cơ bản

1. **brainstorming** — Kích hoạt trước khi viết code. Tinh chỉnh ý tưởng thô qua câu hỏi, khám phá giải pháp thay thế, trình bày thiết kế theo từng phần để validate. Lưu design document.

2. **using-git-worktrees** — Kích hoạt sau khi duyệt thiết kế. Tạo workspace isolated trên branch mới, chạy project setup, xác minh test baseline sạch.

3. **writing-plans** — Kích hoạt khi có thiết kế đã duyệt. Chia nhỏ công việc thành task cỡ vừa (2-5 phút mỗi task). Mỗi task có exact file paths, complete code, verification steps.

4. **subagent-driven-development** hoặc **executing-plans** — Kích hoạt khi có plan. Dispatch subagent mới cho mỗi task với two-stage review (kiểm tra spec compliance, rồi code quality), hoặc thực thi theo batch với human checkpoints.

5. **test-driven-development** — Kích hoạt trong implementation. Bắt buộc RED-GREEN-REFACTOR: viết test fail, xem nó fail, viết code tối thiểu, xem test pass, commit. Xóa code viết trước test.

6. **requesting-code-review** — Kích hoạt giữa các task. Review theo plan, báo cáo issue theo severity. Critical issues chặn tiến trình.

7. **finishing-a-development-branch** — Kích hoạt khi hoàn thành tất cả task. Verify tests, trình bày 4 options (merge/PR/keep/discard), cleanup worktree.

**Agent kiểm tra skills liên quan trước mỗi task.** Đây là mandatory workflows, không phải gợi ý.

## Bên trong có gì

### Thư viện Skills

**Testing**
- **test-driven-development** — Chu trình RED-GREEN-REFACTOR (bao gồm testing anti-patterns reference)

**Debugging**
- **systematic-debugging** — Quy trình 4 pha tìm root cause (bao gồm root-cause-tracing, defense-in-depth, condition-based-waiting)
- **verification-before-completion** — Đảm bảo thực sự đã fix

**Cộng tác**
- **brainstorming** — Tinh chỉnh thiết kế kiểu Socratic
- **writing-plans** — Implementation plans chi tiết
- **executing-plans** — Thực thi theo batch với checkpoints
- **dispatching-parallel-agents** — Workflow subagent song song
- **requesting-code-review** — Checklist trước khi review
- **receiving-code-review** — Phản hồi feedback
- **using-git-worktrees** — Development branches song song
- **finishing-a-development-branch** — Workflow quyết định merge/PR
- **subagent-driven-development** — Iteration nhanh với two-stage review (spec compliance, rồi code quality)

**Meta**
- **writing-skills** — Tạo skills mới theo best practices (bao gồm testing methodology)
- **using-superpowers** — Giới thiệu hệ thống skills

## Triết lý

- **Test-Driven Development** — Viết test trước, luôn luôn
- **Systematic over ad-hoc** — Process thay vì đoán mò
- **Complexity reduction** — Đơn giản là mục tiêu chính
- **Evidence over claims** — Verify trước khi tuyên bố thành công

## Nguồn gốc

LittlePowers là fork của [Superpowers](https://github.com/obra/superpowers) bởi [Jesse Vincent](https://blog.fsck.com) và team tại [Prime Radiant](https://primeradiant.com).

Đọc [bài viết ra mắt gốc](https://blog.fsck.com/2025/10/09/superpowers/).

## Đóng góp

1. Fork repository
2. Chuyển sang branch 'dev'
3. Tạo branch cho công việc của bạn
4. Follow skill `writing-skills` để tạo và test skills mới/đã sửa
5. Gửi PR, nhớ điền template pull request

Xem `skills/writing-skills/SKILL.md` để biết hướng dẫn đầy đủ.

## Cập nhật

Cập nhật LittlePowers phụ thuộc vào coding agent, nhưng thường tự động.

## License

MIT License — xem file LICENSE để biết chi tiết.

## Cộng đồng

- **Discord Superpowers**: [Tham gia](https://discord.gg/35wsABTejz) để hỗ trợ, hỏi đáp, và chia sẻ những gì bạn đang build
- **Issues**: https://github.com/daudaudinang/littlepowers/issues
