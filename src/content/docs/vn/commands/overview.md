---
title: Tham chiếu Lệnh (Command Reference)
description: Các quy ước và tham chiếu chi tiết cho các lệnh của SteelMC.
sidebar:
  order: 1
---

Phần này ghi chép các nhóm lệnh SteelMC cần nhiều hơn một danh sách cú pháp ngắn gọn. Mỗi tham chiếu giải thích những gì một thao tác (operation) thay đổi, nó yêu cầu quyền hạn (permissions) nào và cách sử dụng nó một cách an toàn.

## Danh sách Lệnh

- [`/perms`](../permissions) — kiểm tra và quản lý các quy tắc (rules) của người chơi, nhóm (groups), tính kế thừa (inheritance), mặc định (defaults) và siêu dữ liệu (metadata)

Các lệnh vanilla tuân theo cú pháp Minecraft thông thường của chúng. Cho đến khi một lệnh có hành vi cụ thể của Steel cần tham chiếu riêng, hãy sử dụng [Danh sách lệnh trên Minecraft Wiki](https://minecraft.wiki/w/Commands#List_and_summary_of_commands).

## Cách đọc Cú pháp Lệnh

Tham chiếu lệnh sử dụng dấu ngoặc nhọn cho các giá trị bạn phải cung cấp:

```text
/command <required_value>
```

Không nhập các dấu ngoặc. Ví dụ, `/perms group <group> info` trở thành:

```text
/perms group moderator info
```

Các đối số mục tiêu người chơi có thể chấp nhận tên người chơi hoặc bộ chọn mục tiêu Minecraft tương thích (target selector). Các lệnh sử dụng mục tiêu hồ sơ (profile) cũng có thể giải quyết các người chơi ngoại tuyến đã biết.

## Quyền (Permissions)

Hầu hết các lệnh trích xuất một quyền từ ID lệnh có không gian tên của chúng:

```text
<namespace>.command.<command>
```

Một số nhóm lệnh công bố các quyền cụ thể hơn cho từng hoạt động (operation). Quyền gốc (root permission) cấp các quyền cho những hoạt động đó trừ khi một quy tắc cụ thể hơn từ chối (deny) nó. Giao diện bảng điều khiển (Console) và RCON bỏ qua các kiểm tra quyền của người chơi.

Để biết thông tin chi tiết về các khóa quyền (permission keys), nhóm, ngữ cảnh (contexts) và giải quyết xung đột, hãy xem [Cấu hình Quyền](../../configuration/permissions).
