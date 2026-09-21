# QUY TẮC UI/UX — D2K TECH ENTERPRISE AI PLATFORM

> Source of Truth cho AI khi tạo/chỉnh giao diện. Tất cả tiêu đề và copy người dùng mặc định bằng **tiếng Việt**.

## 1. Tinh thần thiết kế

- Sạch, sáng, chuyên nghiệp, tin cậy.
- Nền chủ đạo trắng; xanh D2K là màu hành động/nhấn mạnh.
- Không biến dashboard thành màn hình monitoring dày đặc kiểu Grafana.
- **Scrolling is acceptable. Visual overload is not.**
- Ưu tiên vertical storytelling: mỗi section trả lời một câu hỏi nghiệp vụ.
- Dùng khoảng trắng để tạo hierarchy thay vì nhiều border, gradient, shadow.

## 2. Màu sắc

```css
:root {
  --d2k-primary: #1677FF;
  --d2k-primary-strong: #0B63E5;
  --d2k-primary-soft: #EAF3FF;
  --d2k-bg: #F7F9FC;
  --d2k-surface: #FFFFFF;
  --d2k-text: #0F1B3D;
  --d2k-text-secondary: #65738B;
  --d2k-border: #E5EBF3;
  --d2k-success: #16B875;
  --d2k-warning: #F5A524;
  --d2k-danger: #E5484D;
  --d2k-purple: #7559E8;
}
```

Không dùng nền tối làm default. Không dùng gradient mạnh cho card/chart. Màu success/warning/danger chỉ để thể hiện trạng thái.

## 3. Typography

- Font đề xuất: `Inter`, fallback system sans-serif; phải render tiếng Việt tốt.
- H1 màn hình: 28–32px, 650–700.
- H2 section: 18–22px, 600–700.
- Card title: 14–16px.
- Body: 14–16px.
- Label/meta: 12–13px.
- Không viết ALL CAPS cho nội dung chính.

## 4. Layout

### 4.1 Shell
- Sidebar trái: 216–240px.
- Header: search + notification + user/workspace.
- Content max-width có thể 1440–1600px trên desktop nhưng giữ padding 24–32px.
- Mobile/tablet chuyển sidebar thành drawer.

### 4.2 Sidebar
Menu chính tối đa 6 mục:
1. Tổng quan
2. Tri thức
3. Tài liệu
4. Trợ lý AI
5. Báo cáo
6. Cài đặt

Các tính năng như Migration, Integration, Workflow, Evaluation là sub-navigation/contextual screen; không đẩy hết ra menu chính.

### 4.3 Dashboard
- Tối đa 4 KPI cards một hàng.
- Mỗi section có một visualization chính.
- Tránh 2 biểu đồ lớn song song.
- Section rộng full content khi dữ liệu quan trọng.
- Cho phép trang dài 2–4 viewport.

Thứ tự gợi ý:
1. Welcome + date filter.
2. KPI summary.
3. Mức độ sử dụng trợ lý AI.
4. Chất lượng tri thức.
5. Tình trạng xử lý tài liệu.
6. Cơ cấu câu hỏi hoặc insight.
7. Tài liệu/hoạt động gần đây.

## 5. Card và surface

- Radius: 10–14px.
- Border: 1px `--d2k-border`.
- Shadow rất nhẹ hoặc không shadow.
- Padding card: 20–24px.
- Không lồng quá 2 tầng card.
- Không dùng nhiều màu nền trong cùng một section.

## 6. Buttons

- Mỗi screen tối đa 1 primary action rõ ràng ở header nếu có.
- Primary: nền xanh, chữ trắng.
- Secondary: nền trắng, border xám/xanh.
- Destructive: đỏ, chỉ khi hành động nguy hiểm.
- Tránh hàng loạt nút nhỏ cùng cấp ưu tiên.

## 7. Tables

- Header xám xanh rất nhạt.
- Row height thoáng 44–52px.
- Trạng thái dùng dot/badge nhẹ.
- Action phụ đưa vào menu `...`.
- Có search/filter ở trên table, không nhồi filter vào từng column nếu không cần.
- Với table dài: sticky header, pagination hoặc virtual scroll.

## 8. Charts

- Mặc định dùng xanh cho primary series, xanh lá cho secondary positive series.
- Grid line nhẹ.
- Không dùng 3D chart.
- Pie/donut chỉ khi category <= 6; nếu nhiều hơn dùng bar/list.
- Tooltip tiếng Việt, có đơn vị và khoảng thời gian.
- Biểu đồ phải có câu hỏi business rõ ràng; không đặt chart chỉ để trang “đẹp”.

## 9. Migration Review screen

Màn hình review ưu tiên đọc/kiểm chứng:
- Stepper ngang ở đầu: Tải lên → Phân tích → Trích xuất → Chuẩn hóa → Kiểm duyệt → Xuất bản.
- Preview tài liệu nguồn chiếm diện tích lớn.
- Bảng fact/field trích xuất ở dưới hoặc section tiếp theo; confidence hiển thị rõ.
- Warning/conflict được gom thành một vùng “Cần kiểm tra”.
- CTA cuối: Yêu cầu chỉnh sửa / Phê duyệt / Xuất bản.
- Không ép mọi panel song song nếu làm nhỏ preview.

## 10. Copywriting

- Tất cả tiêu đề/menu/user-facing label tiếng Việt.
- Dùng thuật ngữ nhất quán: `Kho tri thức`, `Bộ tri thức`, `Tài liệu`, `Trợ lý AI`, `Nguồn dữ liệu`, `Tích hợp`, `Chất lượng tri thức`.
- Có thể giữ tên kỹ thuật/code như `API`, `CRM`, `ERP`, `SKU` khi cần.
- Tránh câu marketing dài trong screen thao tác.

## 11. Trạng thái

Chuẩn badge:
- `Đã xử lý` / `Đang hoạt động`: xanh lá.
- `Đang xử lý`: xanh dương.
- `Cần kiểm tra`: vàng/cam.
- `Lỗi xử lý`: đỏ.
- `Bản nháp`: xám.
- `Đã thay thế`: xám tím nhẹ.

## 12. Responsive

Desktop-first cho Admin Console nhưng không khóa layout 1920px.
- >= 1280: 4 KPI cards.
- 768–1279: 2 KPI cards/row.
- < 768: 1 card/row; sidebar drawer.
- Chart/table phải có fallback phù hợp; không chỉ scale nhỏ vô hạn.

## 13. Accessibility

- Contrast tối thiểu WCAG AA cho text chính.
- Không truyền trạng thái chỉ bằng màu.
- Keyboard focus rõ.
- Icon có aria-label/tooltip nếu không có text.
- Table có header semantics.

## 14. Reference Screens

AI phải tham khảo các ảnh trong `uiux-reference/`:
- `01-dashboard-tong-quan.png`
- `02-kho-tri-thuc.png`
- `03-bao-cao-chat-luong-tri-thuc.png`
- `04-duyet-chuan-hoa-tai-lieu.png`

Ảnh là **style reference**, không phải pixel-perfect spec. Khi rule bằng chữ mâu thuẫn với ảnh, rule trong file này được ưu tiên.

## 15. Do / Don't cho AI

### Do
- Giữ nền trắng/sáng.
- Dùng nhiều whitespace.
- Có một focal point mỗi section.
- Copy tiếng Việt.
- Ưu tiên icon line đơn giản.
- Chấp nhận scroll dài.

### Don't
- Không tạo dark dashboard mặc định.
- Không chia màn hình thành 6–10 widget bằng nhau.
- Không để hai chart lớn cạnh nhau chỉ để tiết kiệm scroll.
- Không tự thêm menu mới nếu chưa có requirement.
- Không tạo hàng chục màu category.
- Không dùng shadow/glassmorphism/gradient mạnh.

## 16. Checklist trước khi merge UI

- [ ] Toàn bộ title/menu/label người dùng là tiếng Việt.
- [ ] Primary action có rõ không?
- [ ] Có quá 4 KPI trong một hàng không?
- [ ] Có section nào chứa quá nhiều mục cạnh tranh sự chú ý không?
- [ ] Có thể tăng whitespace thay vì thêm border không?
- [ ] Chart có trả lời một câu hỏi business không?
- [ ] Mobile/tablet có layout fallback không?
- [ ] Empty/loading/error/permission-denied state đã có chưa?
