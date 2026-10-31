# Hướng Dẫn Cài Đặt Môi Trường VS Code & Biên Dịch Báo Cáo Đồ Án LaTeX

> **Báo cáo Đồ án Java Nâng cao**  
> Khoa Công nghệ Thông tin — Trường Đại học Kiến trúc Đà Nẵng (DAU)

---

## 📌 Mục Lục
1. [Những lỗi đã được khắc phục sẵn](#1-những-lỗi-đã-được-khắc-phục-sẵn)
2. [Cài đặt môi trường trên Visual Studio Code (Từng bước chi tiết)](#2-cài-đặt-môi-trường-trên-visual-studio-code)
   - [Bước 1: Cài đặt bộ phân phối LaTeX (MiKTeX)](#bước-1-cài-đặt-bộ-phân-phối-latex-miktex)
   - [Bước 2: Cài đặt Visual Studio Code & LaTeX Workshop](#bước-2-cài-đặt-visual-studio-code--latex-workshop)
   - [Bước 3: Mở dự án trong VS Code](#bước-3-mở-dự-án-trong-vs-code)
   - [Bước 4: Cài đặt phông chữ Noto Sans (Khuyên dùng)](#bước-4-cài-đặt-phông-chữ-noto-sans-khuyên-dùng)
   - [Bước 5 (Tùy chọn): Cài đặt Perl nếu muốn dùng `latexmk`](#bước-5-tùy-chọn-cài-đặt-perl-nếu-muốn-dùng-latexmk)
3. [Hướng dẫn biên dịch và xem PDF trong VS Code](#3-hướng-dẫn-biên-dịch-và-xem-pdf-trong-vs-code)
4. [Cấu trúc thư mục & Những file cần chỉnh sửa](#4-cấu-trúc-thư-mục--những-file-cần-chỉnh-sửa)
5. [Các lỗi thường gặp và cách xử lý (Troubleshooting)](#5-các-lỗi-thường-gặp-và-cách-xử-lý)

---

## 1. Những lỗi đã được khắc phục sẵn

Trong template ban đầu, một số vấn đề khiến việc biên dịch trên máy cá nhân (đặc biệt là môi trường Windows + VS Code) bị lỗi đã được xử lý:

1. **Lỗi `latexmk: MiKTeX could not find the script engine 'perl'`**:
   - *Nguyên nhân*: Extension LaTeX Workshop trên VS Code mặc định chạy công cụ `latexmk`. Trên Windows, MiKTeX không kèm sẵn Perl nên lệnh này bị lỗi ngay lập tức.
   - *Đã khắc phục*: Thêm cấu hình `.vscode/settings.json` định nghĩa công cụ và chuỗi biên dịch trực tiếp **`XeLaTeX ➞ BibTeX ➞ XeLaTeX ➞ XeLaTeX`** làm mặc định. Chuỗi này chạy 100% bằng binary gốc của TeX/MiKTeX, **không cần cài Perl**.
2. **Lỗi `Missing \begin{document}` khi đang đứng ở file con (`chuong/*.tex`)**:
   - *Nguyên nhân*: LaTeX Workshop tự nhận diện file đang mở làm file gốc để build.
   - *Đã khắc phục*: Cố định root document `"latex-workshop.latex.rootFile.useFile": "do-an.tex"`. Bạn có thể đứng ở bất kỳ file chương nào sửa và nhấn `Ctrl + S`, VS Code đều sẽ tự biên dịch đúng file tổng `do-an.tex`.
3. **Cập nhật thông tin đồ án trong `do-an.tex`**:
   - Khai báo đúng loại báo cáo `\crname{BÁO CÁO ĐỒ ÁN JAVA NÂNG CAO}`, tên đề tài và thông tin sinh viên thực hiện.
4. **Tích hợp xem trước PDF trực tiếp trong VS Code**:
   - Đã cấu hình tab PDF viewer mở song song bên cạnh mã nguồn và kích hoạt đồng bộ hóa SyncTeX hai chiều (bấm vào code nhảy đến trang PDF và ngược lại).

---

## 2. Cài đặt môi trường trên Visual Studio Code

### Bước 1: Cài đặt bộ phân phối LaTeX (MiKTeX)

1. Truy cập trang chủ MiKTeX: [https://miktex.org/download](https://miktex.org/download).
2. Tải về bản **Windows Installer** (file `.exe`).
3. Chạy file cài đặt:
   - Tại bước **"Install missing packages on the fly"**, chọn **`Yes`** (rất quan trọng: tuỳ chọn này giúp MiKTeX tự động tải các gói LaTeX phụ trợ về khi cần mà không ngắt quãng quá trình biên dịch).
4. Sau khi cài xong, mở **Terminal** (PowerShell hoặc CMD) và kiểm tra:
   ```powershell
   xelatex --version
   ```
   Nếu màn hình hiển thị thông tin phiên bản XeTeX/MiKTeX là thành công.

---

### Bước 2: Cài đặt Visual Studio Code & LaTeX Workshop

1. Tải và cài đặt VS Code từ: [https://code.visualstudio.com/](https://code.visualstudio.com/).
2. Mở VS Code, nhấn tổ hợp phím `Ctrl + Shift + X` (mở bảng Quản lý Extension).
3. Gõ tìm kiếm extension: **`LaTeX Workshop`** (tác giả *James Yu*).
4. Bấm nút **Install**.

---

### Bước 3: Mở dự án trong VS Code

1. Trên thanh menu của VS Code, chọn **File** ➔ **Open Folder...**
2. Chọn thư mục chứa đồ án này.
3. Vì dự án đã có sẵn thư mục `.vscode/settings.json`, VS Code sẽ tự động nhận diện mọi cấu hình biên dịch XeLaTeX chuẩn mà bạn **không cần thao tác cài đặt gì thêm**.

---

### Bước 4: Cài đặt phông chữ Noto Sans (Khuyên dùng)

Quy cách định dạng của trường Đại học Kiến trúc Đà Nẵng dùng bộ phông **Noto Sans** và **Noto Sans Mono**. Nếu máy chưa có bộ phông này, tài liệu vẫn biên dịch được nhờ cơ chế fallback sang *Latin Modern*, nhưng có thể phát sinh warning trong log và kiểu chữ không đạt chuẩn nhận diện tốt nhất.

**Cách cài đặt phông Noto Sans trên Windows:**
1. Truy cập và tải 2 bộ phông từ Google Fonts:
   - [Google Fonts - Noto Sans](https://fonts.google.com/specimen/Noto+Sans) ➔ Bấm **Download family** (góc trên bên phải).
   - [Google Fonts - Noto Sans Mono](https://fonts.google.com/specimen/Noto+Sans+Mono) ➔ Bấm **Download family**.
2. Giải nén 2 file `.zip` vừa tải về.
3. Vào các thư mục `static` (hoặc thư mục chứa các file `.ttf`), bôi đen toàn bộ các file font `.ttf`.
4. Nhấp chuột phải, chọn **Install** (hoặc **Install for all users**).

---

### Bước 5 (Tùy chọn): Cài đặt Perl nếu muốn dùng `latexmk`

> [!NOTE]
> Nhờ cấu hình đã có trong `.vscode/settings.json`, bạn **KHÔNG CẦN** bước này vẫn biên dịch bình thường bằng chuỗi `XeLaTeX ➞ BibTeX ➞ XeLaTeX ➞ XeLaTeX`. Bạn chỉ cần cài Perl nếu muốn dùng tiện ích dòng lệnh `latexmk`.

Nếu muốn dùng lệnh `latexmk` trên Terminal:
- Tải và cài đặt **Strawberry Perl** tại [https://strawberryperl.com/](https://strawberryperl.com/) (miễn phí, tự động thêm vào PATH của Windows).
- Hoặc nếu máy bạn đã có **Git for Windows**, bạn chỉ cần thêm thư mục `C:\Program Files\Git\usr\bin` vào biến môi trường `PATH`.

---

## 3. Hướng dẫn biên dịch và xem PDF trong VS Code

### Cách 1: Biên dịch tự động khi lưu (Khuyên dùng)
- Mỗi khi bạn chỉnh sửa nội dung trong bất kỳ file nào (`do-an.tex`, `chuong/*.tex`, `refs.bib`) và bấm **`Ctrl + S`**, LaTeX Workshop sẽ **tự động biên dịch lại** và cập nhật file PDF ngay lập tức.

### Cách 2: Biên dịch thủ công
- Nhấn tổ hợp phím: **`Ctrl + Alt + B`**.
- Hoặc bấm vào biểu tượng chữ **TEX** ở thanh công cụ bên trái VS Code:
  - Mở menu **Commands** ➔ **Build LaTeX project**.
  - Chọn recipe: **`XeLaTeX ➞ BibTeX ➞ XeLaTeX ➞ XeLaTeX`**.

### Cách 3: Mở bản xem trước PDF
- Nhấn tổ hợp phím: **`Ctrl + Alt + V`**.
- Hoặc bấm vào biểu tượng **View LaTeX PDF** (kính lúp có trang tài liệu) ở góc trên bên phải thanh soạn thảo.
- PDF sẽ mở ngay trên một tab bên cạnh file mã nguồn.

### Cách 4: Nhảy qua lại giữa Code và PDF (SyncTeX)
- **Từ code sang PDF**: Đặt con trỏ tại đoạn văn bản trong file `.tex`, nhấn `Ctrl + Alt + J` (hoặc giữ `Ctrl` và bấm chuột trái). Màn hình PDF sẽ tự cuộn đến đúng vị trí tương ứng.
- **Từ PDF sang code**: Giữ phím `Ctrl` và bấm chuột trái vào bất kỳ dòng nào trong tab xem PDF, VS Code sẽ tự động mở đúng file chương và đặt con trỏ tại dòng mã đó.

---

## 4. Cấu trúc thư mục & Những file cần chỉnh sửa

| Thư mục / Tập tin | Được sửa? | Nội dung chi tiết |
|---|:---:|---|
| `do-an.tex` | **SỬA ĐẦU TIÊN** | File chính: khai báo tên đề tài, họ tên sinh viên, MSSV, lớp, GVHD |
| `chuong/*.tex` | **SỬA NHIỀU NHẤT** | Nội dung chi tiết của các chương báo cáo và phụ lục |
| `refs.bib` | **SỬA KHI THÊM TLTK** | Chứa danh mục tài liệu tham khảo theo định dạng BibTeX |
| `figs/` | **BỎ ẢNH VÀO ĐÂY** | Chứa sơ đồ, biểu đồ, ảnh chụp giao diện demo |
| `code/` | **BỎ CODE VÀO ĐÂY** | Mã nguồn mẫu để nhúng vào báo cáo qua `\lstinputlisting` |
| `.vscode/` | **ĐÃ CẤU HÌNH SẴN** | Chứa thiết lập công cụ build XeLaTeX cho VS Code |
| `styles/dau.sty` | **KHÔNG SỬA** | Định dạng màu sắc, phông chữ, hộp, quy chuẩn của DAU |
| `styles/settings.tex`| **KHÔNG SỬA** | Cấu hình lề trang, giãn dòng và nạp gói |

---

## 5. Các lỗi thường gặp và cách xử lý

### 1. Báo cáo bị lỗi font tiếng Việt khi dùng trình biên dịch khác
- **Nguyên nhân**: Mẫu báo cáo dùng gói `fontspec` và `polyglossia`, chỉ tương thích với **XeLaTeX**. Nếu vô tình để VS Code chạy `pdfLaTeX`, bạn sẽ gặp lỗi `Fatal error: fontspec requires XeTeX or LuaTeX`.
- **Cách xử lý**: Luôn kiểm tra dòng đầu tiên của `do-an.tex` có `% !TEX program = xelatex` và chọn đúng recipe XeLaTeX đã cấu hình sẵn trong VS Code.

### 2. Warning: `Package dau Warning: Khong tim thay phong Noto Sans`
- **Nguyên nhân**: Hệ thống chưa cài đặt phông Noto Sans trong máy.
- **Cách xử lý**: Đây là cảnh báo (warning), tài liệu vẫn ra file PDF bình thường bằng font dự phòng Latin Modern. Để tài liệu chuẩn đẹp nhất, hãy làm theo [Bước 4](#bước-4-cài-đặt-phông-chữ-noto-sans-khuyên-dùng) để cài phông.

### 3. File PDF đang mở bằng phần mềm khác nên không ghi đè được
- **Nguyên nhân**: Bạn mở file `do-an.pdf` bằng Acrobat Reader hoặc Foxit Reader khiến file bị khóa (lock), trình biên dịch không thể ghi đè bản mới.
- **Cách xử lý**: Đóng file PDF trên phần mềm ngoài và sử dụng trực tiếp tab xem trước của VS Code (`Ctrl + Alt + V`).
