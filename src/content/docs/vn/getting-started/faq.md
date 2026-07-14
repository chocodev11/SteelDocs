---
title: Câu hỏi thường gặp (FAQ)
description: Giải đáp các câu hỏi thường gặp.
---

Dưới đây bạn sẽ tìm thấy câu trả lời cho những câu hỏi thường gặp nhất!
:::caution[DISCLAIMER]
Steel đang ở giai đoạn tiền alpha (pre-alpha), do đó các tính năng và những câu trả lời dưới đây có thể thay đổi thường xuyên!
:::

## Làm thế nào để cài đặt Steel?

Bạn có thể tìm thấy hướng dẫn đầy đủ [tại đây](../installation).

## Có tệp cấu hình Egg không?

Có, chúng tôi cung cấp tệp cấu hình egg, bạn có thể tìm thêm thông tin trong [hướng dẫn cài đặt](../installation) của chúng tôi.

## Tệp jar (\*.jar) ở đâu?

Steel không được viết bằng Java, vì vậy nó không có tệp jar (\*.jar).\
Đừng lo lắng, bạn vẫn có thể tìm thấy các tệp thực thi cho Windows, Linux và Mac trên [trang tải xuống](../../download) hoặc [GitHub releases](https://github.com/Steel-Foundation/SteelMC/releases).

## Steel có giống với Vanilla (vanilla parity) không?

Ngắn gọn và đơn giản: **Đó là mục tiêu chính của chúng tôi.**

## Steel có plugin không, và plugin Paper/Bukkit của tôi có hoạt động trên Steel không?

Hiện tại thì không. Steel đang trong quá trình phát triển, các plugin đã được lên kế hoạch nhưng chưa thể hoạt động.\
Trong tương lai, Steel sẽ có API riêng, chúng tôi mong muốn mang đến cho nó những phần tốt nhất của API Paper/Bukkit và mod!

## Plugin sẽ được viết bằng ngôn ngữ nào?

API chính sẽ bằng Rust, nhưng cộng đồng có thể thêm hỗ trợ cho WASM hoặc các công nghệ khác để cho phép sử dụng nhiều ngôn ngữ hơn.

## Steel có mod không, và tôi có thể sử dụng các mod hiện tại của mình không?

Các mod NeoForge/Forge/Fabric sẽ không được hỗ trợ. Giống như các plugin, Steel sẽ sử dụng Rust và chức năng nội bộ sẽ khác biệt, do đó những gì có thể làm với nó cũng sẽ khác.

## Làm thế nào để tăng tốc độ bay?

Chỉ cần sử dụng lệnh `/fly speed <velocity multiplier>`, trong đó hệ số nhân vận tốc (velocity multiplier) phải là một số thập phân từ 0 đến 30.

## Khi tôi bay nhanh, máy chủ liên tục kéo tôi lùi lại sau mỗi vài giây

Đó là do một luật chơi (gamerule) của vanilla, bạn có thể sử dụng `/gamerule player_movement_check false` để vô hiệu hóa nó.

## Tôi gặp sự cố, tôi nên làm gì?

Bạn có thể báo cáo trên [GitHub](https://github.com/Steel-Foundation/SteelMC/issues), hoặc tham gia [Discord](/discord) của chúng tôi và đăng nó trong kênh lỗi (bugs).
