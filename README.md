# [Thực hành] Tạo mixins & biến để tối ưu CSS của website mẫu

## 🎯 Mục tiêu
- **Tạo mixins** giúp giảm lặp code CSS.
- **Dùng biến (variables)** để kiểm soát tập trung toàn bộ giao diện (màu sắc, typography, khoảng cách, đổ bóng).
- **Cải thiện tính tái sử dụng và khả năng bảo trì** của CSS thông qua kiến trúc thư mục SASS chuẩn.

---

## 📁 Cấu trúc thư mục dự án

```
sass-mixins-variables-practice/
├── abstracts/
│   ├── _variables.scss      # Khai báo các biến (màu sắc, font size, padding, shadow)
│   └── _mixins.scss         # Khai báo các mixin tái sử dụng (@mixin button-style, @mixin box-style)
├── base/
│   └── _typography.scss     # Định dạng kiểu chữ cơ bản (h1, h2) sử dụng biến
├── components/
│   ├── _buttons.scss        # Định dạng class .button thông qua mixin button-style
│   └── _cards.scss          # Định dạng class .card thông qua mixin box-style
├── main.scss                # File SCSS trung tâm tổng hợp các partials
├── styles.css               # File CSS được biên dịch từ main.scss
├── styles.css.map           # Source map phục vụ debug
├── index.html               # Trang giao diện HTML minh họa kết quả
├── package.json             # Cấu hình npm và scripts biên dịch SASS
└── README.md                # Tài liệu hướng dẫn và báo cáo bài tập
```

---

## 📌 Các bước thực hiện

### Bước 1: File CSS gốc trước khi tối ưu
```css
h1 {
  font-size: 32px;
  color: #222;
}

h2 {
  font-size: 28px;
  color: #222;
}

.button {
  background: red;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
}

.card {
  background: white;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
}
```

### Bước 2: Tạo biến trong SASS (`abstracts/_variables.scss`)
```scss
$primary-color: red;
$text-color: #222;
$font-large: 32px;
$font-medium: 28px;
$padding-standard: 10px 20px;
$box-shadow-default: 2px 2px 10px rgba(0, 0, 0, 0.1);
```

### Bước 3: Tạo mixins tái sử dụng (`abstracts/_mixins.scss`)
```scss
@mixin button-style($bg-color) {
  background: $bg-color;
  color: white;
  padding: $padding-standard;
  border-radius: 5px;
}

@mixin box-style {
  background: white;
  padding: 15px;
  border-radius: 10px;
  box-shadow: $box-shadow-default;
}
```

### Bước 4: Áp dụng mixins và biến vào SCSS
- **`base/_typography.scss`**:
```scss
h1 {
  font-size: $font-large;
  color: $text-color;
}

h2 {
  font-size: $font-medium;
  color: $text-color;
}
```

- **`components/_buttons.scss`**:
```scss
.button {
  @include button-style($primary-color);
}
```

- **`components/_cards.scss`**:
```scss
.card {
  @include box-style;
}
```

- **`main.scss`**:
```scss
@import "abstracts/variables";
@import "abstracts/mixins";
@import "base/typography";
@import "components/buttons";
@import "components/cards";
```

---

## 🛠 Hướng dẫn cài đặt & biên dịch

1. Cài đặt các thư viện cần thiết (nếu muốn dùng lệnh npm cục bộ):
   ```bash
   npm install
   ```

2. Biên dịch SASS sang CSS:
   ```bash
   npm run build:sass
   # hoặc dùng npx trực tiếp:
   npx sass main.scss styles.css
   ```

3. Theo dõi thay đổi tự động (Watch mode):
   ```bash
   npm run watch:sass
   ```

4. Mở file `index.html` trên trình duyệt để kiểm tra kết quả giao diện trực quan.

---

## 🎯 Kết quả đạt được
✔ Tách mã nguồn thành các module độc lập, chuyên biệt theo cấu trúc thư mục rõ ràng.  
✔ CSS đầu ra gọn gàng, kế thừa chính xác theo yêu cầu thiết kế ban đầu.  
✔ Khi thay đổi màu sắc, typography hoặc kích thước, chỉ cần thay đổi tại một điểm duy nhất trong `_variables.scss`.  
✔ Mixin `button-style` và `box-style` có khả năng tái sử dụng linh hoạt cho các thành phần khác.
