# Lab 05 — Builder Track và Capstone Design

**Mục tiêu:** chọn đúng extension surface cho một capstone builder/operator nâng cao, rồi yêu cầu Jarvis viết capstone brief đủ mạnh để triển khai tiếp.  
**Thời lượng:** 45-60 phút.  
**Đọc trước:** `docs/levels/level-5-advanced-builder-capstone.md`

## Lý thuyết cần nắm

Lab này không phải nơi học lại toàn bộ builder internals. Nó chỉ buộc bạn dùng đúng mental model của Level 5 để chọn đúng lớp mở rộng trước khi build.

### Sơ đồ mental model

```text
[Ý tưởng builder]
  -> extension surface nào đúng?
     skill / plugin / provider / memory / API / library
  -> source docs nào phải đọc lại?
  -> assumption kỹ thuật nào phải chốt?
  -> brief có đủ scope + guardrails + next steps chưa?
```

Boundary ngắn cần nhớ:
- Chọn sai extension surface thì code rất nhanh nhưng sai bài.
- Không phải mọi ý tưởng nâng cao đều cần plugin.
- Capstone tốt là capstone có brief rõ, không phải capstone nói to nhưng boundary mơ hồ.

## Hiểu sai thường gặp

- Builder track nghĩa là phải viết plugin ngay.
- Có thể chọn capstone trước rồi mới nghĩ primitive sau.
- Brief capstone chỉ cần ý tưởng hay, không cần assumptions và guardrails.
- Đọc source docs sau khi đã code xong cũng được.

## Prompt lab cho Jarvis

```text
Jarvis, hãy giúp tôi làm Lab 05 về builder capstone design.

Objective:
- Giúp tôi chọn đúng extension surface cho một capstone builder/operator nâng cao.
- Viết một capstone brief ngắn nhưng đủ mạnh để tôi hoặc team triển khai tiếp.

Context:
- Tôi đã đọc `docs/levels/level-5-advanced-builder-capstone.md`.
- Ý tưởng capstone hiện tại của tôi là: <MÔ TẢ Ý TƯỞNG>
- Nếu phù hợp, hãy dùng `docs/templates/capstone-brief-template.md` làm format tham chiếu.

Guardrails:
- Không nhảy thẳng vào implementation.
- Phải chọn rõ extension surface chính.
- Phải chỉ rõ assumptions kỹ thuật và source docs cần đọc lại.
- Nếu ý tưởng đang mơ hồ hoặc chọn sai primitive, hãy sửa framing trước khi viết brief.

Deliverables:
- Quyết định extension surface chính cho capstone.
- Một capstone brief ngắn với scope, constraints, deliverables, risks, và next steps.
- Danh sách source docs / surface cần đọc lại trước khi build thật.

Quality standard:
- Extension surface phải được giải thích rõ vì sao đúng hơn các lựa chọn khác.
- Brief phải đủ cụ thể để Jarvis hoặc team khác có thể tiếp tục.
- Không được dùng ngôn ngữ mơ hồ kiểu “xem sau”, “tùy tình hình” cho phần scope cốt lõi.

Cuối cùng, trả về:
1. Extension surface đã chọn
2. Vì sao không chọn các surface dễ nhầm khác
3. Capstone brief
4. Source docs / assumptions cần đọc lại
5. Điểm nào tôi cần review lại bằng Level 5
```

## Kết quả mong đợi

Output tốt sẽ có:
- quyết định extension surface rõ ràng,
- brief đủ ngắn để đọc nhanh nhưng đủ cụ thể để triển khai tiếp,
- assumptions kỹ thuật và source docs cần đọc lại được liệt kê rõ.

Dấu hiệu cần làm lại:
- vẫn chưa chốt được extension surface chính,
- brief nghe hay nhưng không có scope hoặc rủi ro,
- không nói gì về assumptions,
- dùng plugin như đáp án mặc định cho mọi ý tưởng builder.

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, trước khi đầu tư build một thứ nâng cao trong Hermes, hãy giao cho Jarvis việc chốt đúng extension surface và viết brief trước.

Prompt ngắn để dùng lại:

```text
Jarvis, với ý tưởng này, hãy chốt giúp tôi:
- extension surface đúng,
- assumptions kỹ thuật cần khóa,
- source docs phải đọc lại,
- và một brief đủ mạnh để triển khai tiếp.
```
