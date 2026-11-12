# Prototype — T4 Thương mại điện tử

- **File:** `index.html`. Mở bằng trình duyệt là chạy được ở chế độ **Dựng sẵn**, không cần cài gì.
- **Công cụ:** HTML/CSS/JS thuần, dựng bằng coding agent. Thêm `server.py` (Python 3, chỉ dùng thư viện chuẩn) cho chế độ **AI thật**.
- **Phần mock:** ở chế độ Dựng sẵn, câu trả lời AI viết sẵn theo `spec.md` §3. Ảnh, video, chat với shop luôn là giả.
- **Có lời gọi AI thật (bonus):** ở mức ②, công tắc "AI thật" gửi tin người mua tới `server.py`. Server gọi OpenAI để **đối chiếu khiếu nại với mô tả lúc đặt và chọn bước tiếp theo** (hướng dẫn gửi / xin bằng chứng / chuyển nhân viên / hỏi lại), rồi **kiểm lại theo mức tự chủ** trước khi trả về trình duyệt. Xem mục "Chạy AI thật" bên dưới.
- **Ý chính:** cùng một ca, cùng một model, chỉ đổi **mức tự chủ của tool hoàn tiền**. Đây là tool khó xếp nhất ở `spec.md` §4.4: tool gốc `soan_hoan_tien` của track. Có 3 mức, ứng với ba phương án C / B / A ở §4.4:
  - ① **Cấm (phương án C):** `soan_hoan_tien` bị cấm hẳn, AI chỉ hướng dẫn người mua tự điền khiếu nại.
  - ② **Tách hai tool (phương án B, mức nhóm chọn):** `soan_de_xuat_hoan_tien` AI tự làm (chỉ là bản nháp), `thuc_hien_hoan_tien` cần trưởng nhóm duyệt.
  - ③ **AI tự hoàn tiền (phương án A, quá tay):** `soan_hoan_tien` tự làm.
  - Ở mọi mức: `tu_choi_khieu_nai` cần duyệt, `phat_shop` cấm, còn `tra_cuu_don_hang`, `yeu_cau_bang_chung`, `chuyen_truong_nhom` AI tự làm.
- **Hai ca đắt nhất:** K1 (nồi chiên, shop sửa mô tả sau khi bán) và K4 (tai nghe, shop có thể đúng). Ở mức ③, K1 cho thấy AI "đúng vì may", còn K4 cho thấy AI "hoàn cho xong".

**Phần nhóm tự thêm, không có trong mẫu của giảng viên:**
- Màn hình trưởng nhóm duyệt.
- **Màn hình người bán:** thấy khiếu nại theo mẫu trung lập, cùng bộ căn cứ người mua thấy, đếm ngược 48 giờ. Người bán gửi phản hồi + bằng chứng (đi thẳng vào màn trưởng nhóm) hoặc chấp nhận trả hàng. Không thấy SĐT, địa chỉ hay lời doạ của người mua. Không có nút tự đóng khiếu nại.
- **Lời gọi AI thật + lớp chặn theo mức tự chủ** (`server.py`).
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
| `screen4-nguoi-ban.png` | K1 ở mức ②: màn người bán sau khi người mua gửi yêu cầu (số ⑨, ⑥) |

Số ⑩ (AI thật) chỉ hiện khi chạy `server.py` có key. Bằng chứng nằm ở `ai-log.md`.

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
| 9 | Màn người bán: "Cùng bộ căn cứ người mua đang thấy", hạn 48 giờ, SĐT/địa chỉ ẩn, lời doạ không hiển thị | HAX G11 cho người bán · trung lập (hai bên cùng được nghe) | Người bán tự làm: phản hồi / chấp nhận; không có nút đóng khiếu nại |
| 10 | Nhãn "AI thật · tên model" trên thẻ đối chiếu; nhật ký ghi "Server chặn" khi AI đề xuất tool vượt mức | HAX G2 (nói rõ đây là AI thật) | Cần duyệt / Cấm được **ép ở code**, không chỉ ở prompt |

## Cách bấm khi demo (khoảng 5 phút)

1. Chọn **mức ②** và **K1**. Chỉ vào số 1, 2, 8 trên màn người mua.
2. Bấm **Gửi yêu cầu trả hàng**, rồi **Tôi gửi yêu cầu**. Nhật ký bên phải hiện các tool AI tự gọi.
3. Chuyển sang màn **Trưởng nhóm**. Bấm **Giả lập: shop phản hồi** để hiện đề xuất của AI (chỉ là bản nháp), rồi bấm **Duyệt hoàn tiền**, hộp xác nhận hiện ra (số 6, 7).
4. Chuyển sang **mức ③** và **K4**. Màn hình hiện ô đỏ "Rủi ro của mức này". Đây là lý do nhóm không chọn mức ③.
5. Ca lạ: gõ tin bất kỳ vào ô chat, hoặc bấm các nút trong dòng "Ca lạ".
6. *(Nếu chạy server)* Bật **AI thật**, gõ lại ca lạ: model trả lời thật, còn nhật ký cho thấy server chặn gì (số 10).
7. Màn **Người bán**: sau khi người mua gửi yêu cầu, bấm **Gửi phản hồi + bằng chứng** rồi quay về màn **Trưởng nhóm** để duyệt (số 9).

Mở thẳng một trạng thái bằng đuôi URL: `#k1-2`, `#k4-3`, `#k1-1`, `#k1-2-sent-staff`, `#k1-2-sent-seller` (màn người bán), thêm `-ai` để bật AI thật (ví dụ `#k1-2-ai`).

## Chạy AI thật (bonus)

```powershell
cd prototype
$env:OPENAI_API_KEY = "sk-..."      # chỉ đặt trong cửa sổ terminal này, không ghi vào file nào
python server.py                    # mở http://127.0.0.1:8000
python chay_log.py                  # (cửa sổ khác, cũng đặt key) chạy 8 ca, ghi ai-log.md
```

- Model mặc định là `gpt-4o-mini`. Muốn đổi thì đặt `$env:OPENAI_MODEL`.
- **Không có key trong code, không có key trong log, key không bao giờ gửi về trình duyệt.** `.gitignore` chặn file `.env`. Server cũng không phục vụ `.py` / `.jsonl` qua web.
- Chỉ gửi dữ liệu dựng: `tra_cuu_don_hang()` trả 3 đơn ở `spec.md` §0, đã bỏ số điện thoại shop.
- **Lớp chặn trong `server.py`** (`chan_theo_muc_tu_chu`), tức là lớp 2 ở `spec.md` §4.8:
  - AI đề xuất `thuc_hien_hoan_tien` / `tu_choi_khieu_nai` → **chặn**, đổi thành `chuyen_truong_nhom` + bản nháp.
  - AI đề xuất `phat_shop` → **chặn**.
  - `yeu_cau_bang_chung` → **hoãn** tới khi người mua tự bấm gửi.
  - Câu trả lời có lời hứa hoàn tiền / phạt shop → thay bằng câu an toàn. Có số điện thoại → che đi.
  - Cờ "mô tả sửa sau khi bán" lấy theo **dữ liệu đơn**, không theo model.
  - Ý nào model ghi "lệch" mà **chỉ dựa trên lời kể** (đơn không có lịch sử sửa, shop chưa nhận lỗi) → hạ xuống "chưa kiểm được", độ chắc thấp (K4, K5).
  - Model chọn một đơn mà tin nhắn không nhắc tới (đoán đơn) → bỏ, hỏi lại. Chưa rõ đơn → chỉ cho tra cứu (K2). Trang gửi kèm "đơn đang xem" để các tin nhắn tiếp theo không mất ngữ cảnh.
  - Xin SĐT, đòi khoá shop, tự xưng nhân viên → **câu cố định** do code chèn, kèm luật phát hiện riêng phòng khi model bỏ sót (K6, K7, K9). Doạ bóc phốt → ghi "không vào căn cứ" (K5).
  - Hạn trả hàng và câu "nhân viên sàn quyết" lấy từ **dữ liệu đơn**, không để model tự nhớ (K8).
- *Vì sao phải có các luật này:* lần chạy đầu với `gpt-4o-mini` cho thấy model ghi ANC là "lệch" chỉ dựa trên lời kể, lờ yêu cầu xin SĐT, và quên nhắc hạn. Dặn thêm trong prompt không đảm bảo được, nên các luật này nằm trong code.
- Bằng chứng: `ai-log.md` (8 ca của spec) và `ai-log.jsonl` (output thô của từng lần gọi API). `python chay_log.py --tu-jsonl` dựng lại `ai-log.md` từ output thô đã lưu bằng lớp chặn hiện tại, không gọi lại API.
- Không chạy server thì trang vẫn chạy ở chế độ Dựng sẵn. Bấm "AI thật" sẽ báo lỗi và tự quay về câu dựng sẵn.
