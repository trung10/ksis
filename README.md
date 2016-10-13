# **Tìm hiểu MARKDOWN**
1. Markdown là gì?
Markdown là một ngôn ngữ đánh dấu. Được John Gruber (Với sự góp sức nhà hoạt động Internet Aaron Swartz) tạo ra ngôn ngữ Markdown năm 2004 với mục đích “Dễ viết - dễ đọc - dễ học và có thể chuyển tự động sang tập thẻ HTML (XTHML)”. Markdown được thiết kế để đọc đến đâu hiểu đến đó (khác với chuẩn định dạng RTF và HTML, mã nguồn gốc đọc khá khó hiểu với người bình thường).
Mardown thường được dùng để tạo ra tập tin README, wiki, comment trên nhiều trang web và diễn đàn, viết blog, tin tức,...

2. Các công cụ
Có thể sử dụng bất kỳ trình soạn thảo nào như sublime, notepad, remarkable... để viết Markdown với đuôi file là.md. Bạn có thể viết Markdown online qua các trang web sau:
> https://stackedit.io/editor
> https://jbt.github.io/markdown-editor/
> http://www.tablesgenerator.com/markdown_tables
> https://markable.in/editor/
3. Cú pháp Markdown thường gặp

- Headers
```
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

# H1
## H2
### H3
#### H4
##### H5
###### H6

- In đậm, ghi ngã và gạnh

```
*ngã* hoặc _ngã_
**đậm** hoặc __đậm__
**đậm và _ngã đậm_**
~~Gạnh~~
```
*ngã* hoặc _ngã_
**đậm** hoặc __đậm__
**đậm và _ngã đậm_**
~~Gạnh~~

- Links

```
[từ liên kết](link)
```
[github](https://github.com)

- Chèn hình

```
![ghi chú](link)
```

![hinh](http://s3.amazonaws.com/assets.prod.vetstreet.com/62/44/553732cd42068fe50ca30dfcb592/bulldog-ap-vcr91y-645sm1516.jpg)

- Chèn code bôi đậm

```
Đây là từ `bôi đậm` haha
```
Đây là từ `bôi đậm` haha

![anh](http://me.zing.vn/jpt/photodetail/jee5tee/3228330284)
```
printf("hello world");
```

- Tạo Bảng

```
| Cột 1        | Cột 2       | Cột 3|
| ------------- |:------------:| -----:|
| dòng 1      |       1       |   2   |
| dòng 2      |       3       |   4   |
| dòng 3      |       5       |    6  |
```

| Cột 1        | Cột 2       | Cột 3|
| ------------- |:------------:| -----:|
| dòng 1      |       1       |   2   |
| dòng 2      |       3       |   4   |
| dòng 3      |       5       |    6  |

- Trích dẩn và gạch ngang

```
> Trích dẩn
gạnh
======
```
> Trích dẩn

gạnh
------
Kết luận
======

- Ưu điểm:
Đây là một ngôn ngữ cơ bản dễ dùng giúp ta có thể sử dụng rất nhiều nơi với cú pháp cơ bản. Có lẽ đây là ưu điểm lớn nhất của Markdown.
- Nhược điểm:
Nhược điểm lớn nhất của Markdown có lẽ là khả năng cộng tác trong Markdown và theo giỏi những thay đổi. Vì đơn giản nên chỉ có thể dùng cho các dự án nhỏ lẻ, bài blog và tất nhiên những dự án lớn hay những quyển sách cần nhiều hơn những thứ mà Markdown hỗ trợ.

