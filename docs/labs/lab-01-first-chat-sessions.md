# Lab 01 — First Chat và Session Discipline

**Mục tiêu:** dùng first chat đúng vai trò, hiểu session lifecycle, và biết khi nào nên mở session mới thay vì tiếp tục transcript cũ.  
**Thời lượng:** 20-30 phút.  
**Đọc trước:** `docs/levels/level-1-core-operator.md`

## Lý thuyết cần nắm

Lab này không dạy bạn thuộc lệnh. Lab này chỉ ép bạn dùng đúng boundary giữa single query, interactive session, resume, và profile. Nếu phần này còn mơ hồ, quay lại Level 1 trước khi làm lab.

### Sơ đồ mental model

```text
[Single query]
  -> hỏi nhanh, kiểm base behavior

[Interactive session]
  -> transcript sống cho một workstream

[Resume session]
  -> quay lại đúng workstream cũ

[Profile]
  -> identity/state riêng, KHÔNG phải session
```

Boundary ngắn cần nhớ:
- Dùng single query khi bạn chỉ cần một câu trả lời nhanh.
- Dùng interactive session khi công việc cần nhiều lượt trao đổi.
- Resume khi bạn muốn nối tiếp đúng ngữ cảnh cũ.
- Đổi profile khi cần identity/config khác, không phải chỉ vì muốn chat mới.

## Hiểu sai thường gặp

- Session và profile là một.
- Hễ transcript dài thì phải đổi profile.
- Single query có thể thay thế hoàn toàn session dài.
- Resume một session cũ luôn tốt hơn mở session mới.

## Prompt lab cho Jarvis

Paste prompt này cho Jarvis sau khi bạn đã đọc Level 1:

```text
Jarvis, hãy giúp tôi làm Lab 01 về first chat và session discipline.

Objective:
- Giúp tôi tự phân biệt đúng 4 thứ: single query, interactive session, resume session, profile.
- Không biến lab này thành tutorial dài về command.

Context:
- Tôi đã đọc `docs/levels/level-1-core-operator.md`.
- Tôi đang học cách làm operator đúng boundary trước khi đi tiếp sang level cao hơn.

Guardrails:
- Chỉ dùng giải thích ngắn, không giảng lại cả level.
- Nếu cần command, chỉ nêu command tối thiểu để tôi tự kiểm trên máy.
- Luôn nhắc rõ khi nào tôi nên mở session mới, khi nào nên resume, và khi nào profile mới là đúng.
- Cuối cùng phải để lại một checklist ngắn để tôi dùng lại lần sau.

Deliverables:
- Một bảng so sánh ngắn giữa single query / session / resume / profile.
- 3 tình huống thực tế và quyết định đúng cho mỗi tình huống.
- Một checklist operator ngắn tên gợi ý: `session-discipline-checklist`.

Quality standard:
- Mỗi khái niệm phải có boundary rõ ràng.
- Không được lẫn profile với session.
- Checklist cuối phải đủ ngắn để dùng lại trước mỗi workstream mới.

Cuối cùng, trả về:
1. Tóm tắt boundary
2. 3 tình huống mẫu và quyết định đúng
3. Checklist tái sử dụng
4. Điểm nào tôi nên tự kiểm lại bằng Level 1
```

## Kết quả mong đợi

Output tốt sẽ có:
- giải thích rất ngắn nhưng tách bạch được 4 khái niệm,
- 3 tình huống mẫu kiểu: hỏi nhanh, bugfix nhiều lượt, quay lại việc cũ,
- một checklist ngắn có thể dùng lại trước mỗi phiên làm việc.

Dấu hiệu cần làm lại:
- Jarvis trả lời dài như một lesson mới,
- không nói rõ khi nào mở session mới,
- vẫn lẫn session với profile,
- checklist cuối quá dài hoặc không dùng lại được.

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, trước khi bắt đầu một workstream mới, hãy giao cho Jarvis việc xác định mode làm việc đúng: single query, session mới, hay resume session cũ.

Prompt ngắn để dùng lại:

```text
Jarvis, với việc tôi sắp làm, hãy quyết định giúp tôi:
- nên dùng single query, mở session mới, hay resume session cũ,
- có cần profile khác không,
- và giải thích ngắn trong 5 dòng vì sao.
```
