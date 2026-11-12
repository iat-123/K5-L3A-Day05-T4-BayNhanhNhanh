# Prototype — T4 Thương mại điện tử

- **File:** `index.html`. Mở bằng trình duyệt, không cần cài gì.
- **Công cụ:** HTML/CSS/JS thuần, dựng bằng coding agent.
- **Phần mock:** toàn bộ câu trả lời AI được viết sẵn theo `spec.md` §3; ảnh, video, chat với shop đều là giả. **Không có lời gọi AI thật**, không có API key.
- **Ý chính:** cùng một ca, cùng một model, chỉ đổi **mức tự chủ của `soan_hoan_tien`**, tool khó xếp nhất ở §4. Có 3 mức:
  - ① **Cấm:** AI chỉ hướng dẫn.
  - ② **AI soạn, trưởng nhóm duyệt:** mức nhóm chọn.
  - ③ **AI tự hoàn tiền:** mức quá tay.
- **Hai ca đắt nhất:** K1 (nồi chiên, shop sửa mô tả sau khi bán) và K4 (tai nghe, shop có thể đúng). Ở mức ③, K1 cho thấy AI "đúng vì may", còn K4 cho thấy AI "hoàn cho xong".

**Phần nhóm tự thêm, không có trong mẫu của giảng viên:**
- Màn hình trưởng nhóm duyệt.
- Chuyển giữa 2 ca K1 và K4.
- Nhật ký các tool AI đã gọi.
- Đếm ngược hạn trả hàng, gửi yêu cầu xong thì hạn được giữ.
- Ô chat và các nút thử ca lạ.

## Ảnh

| Ảnh | Nội dung |
|---|---|
| `screen1-muc2-nguoi-mua.png` | K1 ở mức ②: màn người mua |
| `screen2-muc2-truong-nhom.png` | K1 ở mức ②: màn trưởng nhóm, sau khi người mua đã gửi yêu cầu và shop đã phản hồi |
| `screen3-muc3-rui-ro.png` | K4 ở mức ③: AI tự hoàn tiền, kèm ô "Rủi ro của mức này" |

## Các số trên ảnh

| Số | Thể hiện gì | Nguyên tắc | Mức tự chủ |
|---|---|---|---|
| 1 | Thẻ so sánh mô tả lúc đặt / hiện tại, tô chỗ shop sửa (K1); thẻ đối chiếu ✔ / ? (K4) | G11 · giải thích vì sao | — |
| 2 | Dòng "Căn cứ", nhãn độ chắc, thanh xanh "đang chat với AI, AI không quyết định hoàn tiền" | G2 + PAIR Explainability & Trust | — |
| 3 | "Chưa đủ căn cứ" + xin đúng một bằng chứng (K4); hỏi lại một câu kèm nút chọn đơn | G10 · thu hẹp khi nghi ngờ | — |
| 4 | Nút "Mô tả này không giống những gì tôi thấy" | G9 · sửa dễ | — |
| 5 | Nút "Gặp nhân viên" + "Huỷ chuyển" | PAIR Errors + Graceful Failure | Tự làm, có hoàn tác |
| 6 | Nút hoàn tiền / từ chối chỉ ở màn trưởng nhóm, có hộp xác nhận | — | Cần duyệt |
| 7 | Không có nút phạt shop; bị đòi khoá shop thì AI không hứa | — | Cấm |
| 8 | Đếm ngược hạn trả hàng → "Hạn được giữ ✓" | — | — |

## Cách bấm khi demo (khoảng 4 phút)

1. Chọn **mức ②** và **K1**. Chỉ vào số 1, 2, 8 trên màn người mua.
2. Bấm **Gửi yêu cầu trả hàng**, rồi **Tôi gửi yêu cầu**. Nhật ký bên phải hiện các tool AI tự gọi.
3. Chuyển sang màn **Trưởng nhóm**. Bấm **Giả lập: shop phản hồi** để hiện đề xuất của AI (chỉ là bản nháp), rồi bấm **Duyệt hoàn tiền**, hộp xác nhận hiện ra (số 6, 7).
4. Chuyển sang **mức ③** và **K4**. Màn hình hiện ô đỏ "Rủi ro của mức này". Đây là lý do nhóm không chọn mức ③.
5. Ca lạ: gõ tin bất kỳ vào ô chat, hoặc bấm các nút trong dòng "Ca lạ".

Mở thẳng một trạng thái bằng đuôi URL: `#k1-2`, `#k4-3`, `#k1-1`, `#k1-2-sent-staff`.
