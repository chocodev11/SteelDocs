---
title: Các lệnh về Quyền
description: Kiểm tra và quản lý các quyền của SteelMC, nhóm, tính kế thừa và siêu dữ liệu với lệnh /perms.
sidebar:
  order: 2
---

`/perms` quản lý các quyền trong khi máy chủ đang chạy. Nó có ba phạm vi (scopes):

```text
/perms user <targets> <operation>
/perms group <group> <operation>
/perms groups <operation>
```

- `user` quản lý các ghi đè trực tiếp của người chơi và việc gán nhóm.
- `group` quản lý các quy tắc, mức độ ưu tiên, tính kế thừa và siêu dữ liệu của một nhóm được đặt tên.
- `groups` liệt kê các nhóm và quản lý những nhóm nào mà mọi người chơi nhận được theo mặc định.

Tham chiếu này giả định rằng bạn đã quen thuộc với các khóa quyền (permission keys), ký tự đại diện (wildcards), nhóm và ngữ cảnh (contexts). Hãy bắt đầu với [Cấu hình Quyền](../../configuration/permissions) nếu những khái niệm này mới đối với bạn.

:::tip
Hãy kiểm tra (inspect) một người chơi hoặc một nhóm trước khi chỉnh sửa nó. Các quy tắc trực tiếp, quy tắc kế thừa, quy tắc theo ngữ cảnh và mức độ ưu tiên của nhóm có thể làm cho kết quả có hiệu lực khác với một mục nhập cấu hình duy nhất.
:::

## Cách Ủy quyền (Authorization) hoạt động

Mỗi hoạt động (operation) có một lệnh quyền (command permission) chi tiết. Ví dụ: kiểm tra người chơi yêu cầu `steel.command.perms.user.info`. Quyền gốc `steel.command.perms` cấp mọi hoạt động của lệnh `/perms` trừ khi một hoạt động cụ thể hơn bị từ chối (deny).

Việc thay đổi hoặc hiển thị dữ liệu được quản lý có thể yêu cầu thêm quyền (authority):

| Quyền hạn | Mục đích |
| --- | --- |
| `steel.permission.manage.<permission>` | Kiểm tra, xem, hoặc thay đổi các quy tắc đối với khóa quyền đó |
| `steel.permission.group.<group>` | Kiểm tra, chỉ định hoặc thay đổi nhóm đó |
| `steel.permission.metadata` | Hiển thị, kiểm tra hoặc thay đổi siêu dữ liệu của quyền |

Ký tự đại diện có thể cấp một phạm vi quyền hạn. Ví dụ: `steel.permission.manage.minecraft.command.*` cho phép quản lý các khóa con (descendant keys) như `minecraft.command.give`, trong khi `steel.permission.manage.*` cho phép quản lý mọi khóa quyền. Tương tự, `steel.permission.group.*` cho phép quản lý mọi nhóm.

Các lệnh thông tin (Information commands) lọc đầu ra của chúng theo thẩm quyền của người gửi. Ví dụ: thông tin người dùng bỏ qua siêu dữ liệu nếu không có `steel.permission.metadata` và bỏ qua các quy tắc hoặc nhóm mà người gửi không thể quản lý.

Console và RCON bỏ qua các kiểm tra quyền của người chơi này.

## Kiểm tra Cài đặt Hiện tại

Đây là những lệnh tốt nhất nên chạy trước khi thực hiện các thay đổi.

### Kiểm tra một người chơi

```text
/perms user <targets> info
```

Hiển thị các nhóm được gán cho mỗi mục tiêu, các quy tắc quyền trực tiếp và các ghi đè siêu dữ liệu trực tiếp. Điều này báo cáo các cài đặt người chơi được lưu trữ, không phải mọi quy tắc có hiệu lực được kế thừa.

Lệnh quyền: `steel.command.perms.user.info`

Đầu ra được lọc bởi quản lý quyền, quản lý nhóm và quyền siêu dữ liệu của người gửi.

```text
/perms user Steve info
```

### Kiểm tra quyền có hiệu lực của một người chơi

```text
/perms user <targets> check <permission_expr>
```

Giải quyết quyền cho mỗi mục tiêu, bao gồm các nhóm mặc định, các nhóm được gán, các ghi đè trực tiếp, tính kế thừa, mức độ ưu tiên và ngữ cảnh. Kết quả xác định quy tắc chiến thắng (winning rule) và nguồn của nó, hoặc báo cáo rằng quyền chưa được đặt.

Lệnh quyền: `steel.command.perms.user.check`

Quyền hạn bổ sung: `steel.permission.manage.<permission>` đối với khóa được kiểm tra.

```text
/perms user Steve check minecraft.command.gamemode.creative
/perms user Steve check minecraft.command.gamemode{domain=lobby}
```

### Kiểm tra một nhóm

```text
/perms group <group> info
```

Hiển thị mức độ ưu tiên của nhóm, các nhóm cha, các quy tắc cho phép (allow rules), quy tắc từ chối (deny rules) và siêu dữ liệu. Các mục nằm ngoài thẩm quyền quản lý của người gửi sẽ bị bỏ qua.

Lệnh quyền: `steel.command.perms.group.info`

Quyền hạn bổ sung: `steel.permission.group.<group>`. Siêu dữ liệu cũng yêu cầu `steel.permission.metadata`; các quy tắc quyền riêng lẻ yêu cầu quyền quản lý tương ứng `steel.permission.manage.<permission>`.

```text
/perms group moderator info
```

### Liệt kê các nhóm

```text
/perms groups list
```

Liệt kê các nhóm mà người gửi có quyền quản lý và cho biết nhóm nào là nhóm mặc định.

Lệnh quyền: `steel.command.perms.groups.list`

Chỉ các nhóm nằm trong quyền quản lý nhóm của người gửi `steel.permission.group.<group>` mới được hiển thị.

## Quản lý Quy tắc Quyền của Người chơi

Biểu thức quyền (permission expression) là một khóa quyền với bộ chọn ngữ cảnh tùy chọn:

```text
<permission>{<context>=<value>,...}
```

Xem [Quy tắc theo ngữ cảnh](../../configuration/permissions#contextual-rules) để biết cú pháp hoàn chỉnh và hành vi giải quyết.

### Cho phép (Allow) một quyền

```text
/perms user <targets> allow <permission_expr>
```

Thêm hoặc thay thế chính xác quy tắc trực tiếp bằng một quyền cho phép. Các ngữ cảnh khác cho cùng một quyền vẫn không thay đổi.

Lệnh quyền: `steel.command.perms.user.allow`

Quyền hạn bổ sung: `steel.permission.manage.<permission>`.

```text
/perms user Steve allow minecraft.command.teleport
/perms user Steve allow minecraft.command.gamemode{domain=lobby}
```

### Từ chối (Deny) một quyền

```text
/perms user <targets> deny <permission_expr>
```

Thêm hoặc thay thế chính xác quy tắc trực tiếp bằng một quyền từ chối. Một sự từ chối cụ thể có thể ghi đè một sự cho phép rộng hơn.

Lệnh quyền: `steel.command.perms.user.deny`

Quyền hạn bổ sung: `steel.permission.manage.<permission>`.

```text
/perms user Steve deny minecraft.command.gamemode.creative
```

### Xóa bỏ (Unset) một quy tắc trực tiếp

```text
/perms user <targets> unset <permission_expr>
```

Chỉ loại bỏ quy tắc trực tiếp có cùng khóa quyền và ngữ cảnh chính xác. Các quy tắc kế thừa từ nhóm không bị loại bỏ.

Lệnh quyền: `steel.command.perms.user.unset`

Quyền hạn bổ sung: `steel.permission.manage.<permission>`.

```text
/perms user Steve unset minecraft.command.gamemode{domain=lobby}
```

## Gán các Nhóm Người chơi

### Thêm một nhóm

```text
/perms user <targets> group add <group>
```

Gán nhóm cho mỗi mục tiêu. Việc gán một nhóm đã hiện diện sẽ không tạo ra thay đổi nào.

Lệnh quyền: `steel.command.perms.user.group.add`

Quyền hạn bổ sung: `steel.permission.group.<group>`.

```text
/perms user Steve group add moderator
```

### Xóa một nhóm

```text
/perms user <targets> group remove <group>
```

Loại bỏ nhóm được gán khỏi mỗi mục tiêu. Điều này không xóa một nhóm nhận được thông qua `default_groups` hoặc kế thừa.

Lệnh quyền: `steel.command.perms.user.group.remove`

Quyền hạn bổ sung: `steel.permission.group.<group>`.

```text
/perms user Steve group remove moderator
```

## Quản lý Nhóm

### Tạo hoặc xóa một nhóm

```text
/perms group <group> create
/perms group <group> delete
```

`create` thêm một nhóm trống. `delete` xóa nhóm khỏi cấu hình nhóm. Steel từ chối việc xóa nếu nhóm là `op`, vẫn là nhóm mặc định hoặc được một nhóm khác kế thừa; hãy xóa các tham chiếu đó trước.

Lệnh quyền: `steel.command.perms.group.create` và `steel.command.perms.group.delete`.

Quyền hạn bổ sung: `steel.permission.group.<group>`.

```text
/perms group moderator create
/perms group retired-staff delete
```

### Cho phép, từ chối, hoặc hủy bỏ một quy tắc nhóm

```text
/perms group <group> allow <permission_expr>
/perms group <group> deny <permission_expr>
/perms group <group> unset <permission_expr>
```

`allow` và `deny` thêm hoặc thay thế quy tắc nhóm chính xác. `unset` chỉ xóa quy tắc có cùng khóa quyền và ngữ cảnh chính xác.

Lệnh quyền:

- `steel.command.perms.group.allow`
- `steel.command.perms.group.deny`
- `steel.command.perms.group.unset`

Quyền hạn bổ sung: Cả `steel.permission.group.<group>` và `steel.permission.manage.<permission>`.

```text
/perms group moderator allow minecraft.command.teleport
/perms group moderator deny minecraft.command.stop
/perms group moderator unset minecraft.command.teleport
```

### Thay đổi mức độ ưu tiên của nhóm

```text
/perms group <group> priority <priority>
```

Thiết lập mức độ ưu tiên số nguyên có dấu 32-bit được sử dụng khi các quy tắc có độ cụ thể như nhau từ các nhóm khác nhau bị xung đột. Mức độ ưu tiên cao hơn sẽ chiến thắng; độ cụ thể vẫn được ưu tiên hơn mức độ ưu tiên.

Lệnh quyền: `steel.command.perms.group.priority`

Quyền hạn bổ sung: `steel.permission.group.<group>`.

```text
/perms group moderator priority 10
```

## Quản lý Tính kế thừa của Nhóm

### Liệt kê các nhóm cha

```text
/perms group <group> inherit list
```

Liệt kê các nhóm cha trực tiếp của nhóm. Các mục nhóm cha nằm ngoài thẩm quyền quản lý nhóm của người gửi sẽ bị bỏ qua.

Lệnh quyền: `steel.command.perms.group.inherit.list`

Quyền hạn bổ sung: `steel.permission.group.<group>`.

```text
/perms group moderator inherit list
```

### Thêm hoặc xóa một nhóm cha

```text
/perms group <group> inherit add <parent>
/perms group <group> inherit remove <parent>
```

Thêm hoặc loại bỏ một mối quan hệ kế thừa trực tiếp. Việc thêm một mối quan hệ tạo ra một chu kỳ sẽ bị từ chối. Các quy tắc được kế thừa sẽ giữ mức ưu tiên của nhóm nơi chúng được định nghĩa.

Lệnh quyền: `steel.command.perms.group.inherit.add` và `steel.command.perms.group.inherit.remove`.

Quyền hạn bổ sung: `steel.permission.group.<group>` và `steel.permission.group.<parent>`.

```text
/perms group moderator inherit add helper
/perms group moderator inherit remove helper
```

## Quản lý Siêu dữ liệu (Metadata)

Biểu thức siêu dữ liệu (Metadata expressions) sử dụng khóa có không gian tên (namespaced key) và cú pháp ngữ cảnh tùy chọn giống như các biểu thức quyền:

```text
<namespace>:<key>{<context>=<value>,...}
```

Các giá trị được đánh máy (typed) rõ ràng dưới dạng `int`, `bool`, hoặc `string`. Hãy xem [Siêu dữ liệu](../../configuration/permissions#metadata) để biết các quy tắc giải quyết.

Trích dẫn các giá trị chuỗi có chứa khoảng trắng:

```text
/perms user Steve metadata set string "Senior Builder" plugin:rank
```

### Đặt siêu dữ liệu người chơi

```text
/perms user <targets> metadata set int <value> <metadata_expr>
/perms user <targets> metadata set bool <value> <metadata_expr>
/perms user <targets> metadata set string <value> <metadata_expr>
```

Đặt ghi đè siêu dữ liệu trực tiếp cho từng mục tiêu. Nó chỉ thay thế giá trị có cùng khóa siêu dữ liệu và ngữ cảnh chính xác.

Lệnh quyền: `steel.command.perms.user.metadata.set`

Quyền hạn bổ sung: `steel.permission.metadata`.

```text
/perms user Steve metadata set int 5 plugin:max_homes
/perms user Steve metadata set string gold plugin:chat/color{domain=lobby}
```

### Kiểm tra siêu dữ liệu người chơi

```text
/perms user <targets> metadata check <metadata_expr>
```

Giải quyết giá trị có hiệu lực cho mỗi mục tiêu tại ngữ cảnh được yêu cầu và báo cáo nguồn chiến thắng. Điều này bao gồm siêu dữ liệu của nhóm và các ghi đè trực tiếp.

Lệnh quyền: `steel.command.perms.user.metadata.check`

Quyền hạn bổ sung: `steel.permission.metadata`.

```text
/perms user Steve metadata check plugin:max_homes{world=survival:overworld}
```

### Xóa siêu dữ liệu người chơi

```text
/perms user <targets> metadata unset <metadata_expr>
```

Chỉ xóa mục nhập siêu dữ liệu trực tiếp có cùng khóa và ngữ cảnh chính xác. Siêu dữ liệu kế thừa từ nhóm vẫn không thay đổi.

Lệnh quyền: `steel.command.perms.user.metadata.unset`

Quyền hạn bổ sung: `steel.permission.metadata`.

```text
/perms user Steve metadata unset plugin:max_homes
```

### Đặt hoặc xóa siêu dữ liệu nhóm

```text
/perms group <group> metadata set int <value> <metadata_expr>
/perms group <group> metadata set bool <value> <metadata_expr>
/perms group <group> metadata set string <value> <metadata_expr>
/perms group <group> metadata unset <metadata_expr>
```

Đặt hoặc loại bỏ một mục siêu dữ liệu chính xác trong nhóm. Sử dụng `/perms group <group> info` để kiểm tra cấu hình siêu dữ liệu của nhóm; không có thao tác `check` siêu dữ liệu riêng biệt cho nhóm.

Lệnh quyền: `steel.command.perms.group.metadata.set` và `steel.command.perms.group.metadata.unset`.

Quyền hạn bổ sung: `steel.permission.group.<group>` và `steel.permission.metadata`.

```text
/perms group vip metadata set int 10 plugin:max_homes
/perms group vip metadata unset plugin:max_homes
```

## Quản lý các Nhóm Mặc định (Default Groups)

```text
/perms groups default add <group>
/perms groups default remove <group>
```

Thêm hoặc loại bỏ một nhóm khỏi `default_groups`. Các nhóm mặc định đóng góp vào quyền và siêu dữ liệu có hiệu lực của mọi người chơi. Việc xóa nhóm mặc định sẽ không xóa bản thân nhóm đó.

Lệnh quyền: `steel.command.perms.groups.default.add` và `steel.command.perms.groups.default.remove`.

Quyền hạn bổ sung: `steel.permission.group.<group>`.

```text
/perms groups default add member
/perms groups default remove member
```

## Tính bền vững và Người chơi Ngoại tuyến

Các lệnh về nhóm cập nhật `config/groups.toml`. Các lệnh về người chơi cập nhật `<save_root>/global/player_permissions.toml` và có thể giải quyết các người chơi đang trực tuyến, người chơi ngoại tuyến đã biết hoặc tên hồ sơ có sẵn thông qua tra cứu hồ sơ (profile lookup) của máy chủ.

Các lệnh này chạy mà không chặn quá trình tick (chu kỳ) của máy chủ. Quá trình thực thi lệnh sẽ vẫn bị treo cho đến khi thao tác kết thúc, sau đó người gửi sẽ nhận được kết quả và phản hồi của lệnh.
