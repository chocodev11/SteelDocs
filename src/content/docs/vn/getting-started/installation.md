---
title: Cài đặt
description: Cách cài đặt Steel trên hệ thống của bạn.
sidebar:
  order: 1
---

Hướng dẫn này sẽ chỉ cho bạn cách cài đặt và chạy Steel.\
Có nhiều cách để làm điều đó:

- [Tệp nhị phân có sẵn](#pre-built-binaries)
- [Docker](#docker)
- [Egg (Pelican / Pterodactyl)](#egg)
- [Tự biên dịch](#self-compilation)

## Chọn phương pháp nào

Bạn nên chọn phương pháp cài đặt phù hợp với nơi Steel sẽ chạy.

- Nếu bạn muốn chạy Steel trực tiếp trên hệ thống máy chủ (host system), hãy sử dụng các tệp nhị phân có sẵn.

- Nếu bạn đã sử dụng Kubernetes, Docker Swarm hoặc một máy chủ Docker tiêu chuẩn, bộ chứa (container) Docker là lựa chọn được khuyến nghị.

- Nếu nền tảng mục tiêu của bạn là Pelican, Pterodactyl hoặc một bảng điều khiển dựa trên egg khác, hãy sử dụng Steel egg được cung cấp ở định dạng JSON hoặc YAML.

- Nếu bạn muốn tốc độ bộ xử lý nguyên bản hoặc nền tảng/kiến trúc của bạn hiện không được hỗ trợ, hãy xây dựng Steel từ mã nguồn cho hệ thống mục tiêu của bạn.

| Phương pháp                     | Linux x64 | Linux arm64 | Linux armv7 | Linux armv6 | Windows x64 | Windows arm | Mac arm | Mac x64 |
| ------------------------------- | :-------: | :---------: | :---------: | :---------: | :---------: | :---------: | :-----: | :-----: |
| [Nhị phân](#pre-built-binaries) |    ✅     |     ❌      |     ❌      |     ❌      |     ✅      |     ❌      |   ✅    |   ❌    |
| [Docker](#docker)               |    ✅     |     ✅      |     ❌      |     ❌      |     ✅      |     ✅      |   ✅    |   ✅    |
| [Egg](#egg)                     |    ✅     |     ❌      |     ❌      |     ❌      |     ❌      |     ❌      |   ❌    |   ❌    |
| [Tự biên dịch](#self-compilation)|    ✅     |     ✅      |     ⚠️      |     ⚠️      |     ✅      |     ✅      |   ✅    |   ✅    |

✅: Được hỗ trợ. ❌: Hiện không được hỗ trợ. ⚠️: Cần điều chỉnh nhỏ.

Các phần bên dưới mô tả chi tiết từng tùy chọn.

## Tệp nhị phân có sẵn (Pre-built Binaries)

Các tệp nhị phân có sẵn có thể tìm thấy trên [trang tải xuống](../../download) của chúng tôi cho các nền tảng sau:

- Linux (x86_64)
- Windows (x86_64)
- MacOS (ARM)

Ngoài ra, trang [GitHub releases](https://github.com/Steel-Foundation/SteelMC/releases) của chúng tôi cũng có sẵn các tệp tương tự.

Sau khi tải xuống, để chạy máy chủ, bạn chỉ cần mở thiết bị đầu cuối (terminal) ưa thích của mình (ví dụ: PowerShell, Ghostty, Kitty, v.v.) trong thư mục chứa tệp thực thi đã tải xuống và chạy lệnh sau:

```bash
# Windows
./steel.exe

# MacOS / Linux
./steel
```

:::caution
Đối với người dùng MacOS, bạn cần bật Chế độ dành cho nhà phát triển (Development Mode) của hệ thống để có thể chạy Steel, vì nó không được ký chính thức.

Có thể thực hiện điều này bằng cách chạy lệnh sau trong terminal và nhập mật khẩu quản trị viên của bạn:

```bash
sudo devtools enable
```

:::

## Docker

Steel không xuất bản thẻ Docker `latest`. Hãy sử dụng thẻ phiên bản cụ thể để thay thế, theo cách này, các bản nâng cấp luôn được lên kế hoạch và không có bộ chứa nào cập nhật ngoài dự kiến.

Thẻ `nightly` có sẵn để thử nghiệm plugin và thử nghiệm sớm các thay đổi sắp tới. Nó theo sát commit mới nhất trên nhánh `master` và có thể chứa các lỗi chưa được sửa chữa cho một bản phát hành chính thức. Đừng sử dụng `nightly` trong môi trường sản xuất.

### Khởi động image

Chạy bộ chứa Steel với cổng máy chủ được phơi bày (exposed) và gắn (mount) các thư mục cục bộ cho cấu hình và dữ liệu lưu:

```bash
docker run -d -p 25565:25565 -v ./config:/config -v ./saves:/saves -v ./logs:/logs ghcr.io/steel-foundation/steelmc:<version>
```

:::note
Lệnh docker pull cho phiên bản nightly là: `docker pull ghcr.io/steel-foundation/steelmc:nightly`
:::

:::tip
Thiết lập tương tự có thể được viết thành dịch vụ Docker Compose (Được khuyến nghị):

```yaml
# docker-compose.yml
services:
  steel:
    image: ghcr.io/steel-foundation/steelmc:<version>
    ports:
      - 25565:25565
    volumes:
      - ./config:/config
      - ./saves:/saves
      - ./logs:/logs
```

Docker Compose là một cách khác để viết lệnh `docker run` vào một tệp.\
Bạn có thể tìm thấy tài liệu tham khảo đầy đủ về Docker Compose tại đây: https://docs.docker.com/reference/compose-file/

:::

### Cách thay đổi cổng và thư mục

Đối với những người không có khái niệm về cách cấu hình docker, đây là một bảng chú giải nhỏ về các tham số có thể định cấu hình:

- **`-p` (`--port`, `ports`):** Số đầu tiên đại diện cho cổng máy chủ lưu trữ (host port), có thể được thay đổi. Số thứ hai là cổng bên trong bộ chứa Docker và chỉ nên được thay đổi để khớp với cổng trong cấu hình Steel. Ví dụ: `-p 1111:25565` giúp máy chủ Minecraft có thể truy cập được trên cổng `1111` trên máy chủ Linux.
- **`-v` (`--volume`, `volumes`):** Đường dẫn trước dấu hai chấm là đường dẫn trên hệ thống máy chủ của bạn, có thể liên quan (relative) đến tệp compose. Đường dẫn sau dấu hai chấm là bên trong bộ chứa Docker và không nên thay đổi.

## Egg

Đối với Pelican và Pterodactyl, Steel cung cấp một egg có thể được nhập trực tiếp từ cửa hàng egg của bảng điều khiển.

Đối với các nền tảng dựa trên egg khác, hãy sử dụng tệp egg JSON hoặc YAML từ kho lưu trữ của chúng tôi hoặc tải xuống egg phù hợp từ một trong các cửa hàng được hỗ trợ khi có sẵn.

Pelican: `[Sắp ra mắt]`

Pterodactyl: `[Sắp ra mắt]`

## Tự biên dịch (Self Compilation)

### Yêu cầu

- **Chuỗi công cụ Rust nightly** - Steel sử dụng các tính năng của Rust phiên bản 2024
- **Hệ điều hành 64-bit** - Linux, MacOS hoặc Windows
- **Git** - Để sao chép (clone) kho lưu trữ

### Cài đặt Rust

Nếu bạn chưa cài đặt Rust, hãy sử dụng [rustup](https://rustup.rs/).

```bash
# For Unix systems only (e.g. MacOS, Linux)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Steel yêu cầu chuỗi công cụ nightly. Kho lưu trữ bao gồm một tệp `rust-toolchain.toml` sẽ tự động chọn phiên bản chính xác.

### Xây dựng từ Mã nguồn

```bash
# Clone the repository
git clone https://github.com/Steel-Foundation/SteelMC.git
cd SteelMC

# Build in release mode (recommended for running)
cargo build-native --release
```

:::note
**Tệp nhị phân sẽ nằm ở `./target/release/steel`**
:::

:::caution
Nếu bạn muốn xây dựng cho armv6 và armv7. Bạn cũng cần sao chép các thay đổi simdnbt trong Cargo.toml để sử dụng phiên bản cục bộ của bạn và xóa giới hạn 64bit khỏi simdnbt.
Để được trợ giúp thêm, chúng tôi có thể hỗ trợ tại [discord](/discord).
:::

#### Các lệnh biên dịch

| Lệnh                           | Mục đích                                                                                |
| ------------------------------ | --------------------------------------------------------------------------------------- |
| `cargo build-native`           | Bản dựng Debug (biên dịch nhanh hơn, chạy chậm hơn) (có tối ưu hóa theo kiến trúc)      |
| `cargo build-native --release` | Bản dựng Release (biên dịch chậm hơn, được tối ưu hóa) (có tối ưu hóa theo kiến trúc)   |
| `cargo build`                  | Bản dựng Debug (biên dịch nhanh hơn, chạy chậm hơn)                                     |
| `cargo build --release`        | Bản dựng Release (biên dịch chậm hơn, được tối ưu hóa)                                  |
| `cargo check`                  | Kiểm tra cú pháp và kiểu (type checking) nhanh                                          |
| `cargo test`                   | Chạy bộ thử nghiệm (test suite)                                                         |
| `cargo clippy`                 | Chạy bộ linter                                                                          |

:::caution
Các bản dựng Native không thể được thực thi trên các máy khác, chỉ trên cùng máy mà chúng được biên dịch.
(Hoặc một máy có cùng kiến trúc)
:::

#### Tính năng biên dịch (Build Features)

Steel hỗ trợ các tính năng xây dựng tùy chọn có thể được bật bằng `--features`:

```bash
# Enable deadlock detection (debug only)
cargo build-native --features deadlock_detection

# Enable heap profiling with dhat
cargo build-native --features dhat-heap

# Disable the default mimalloc allocator
cargo build-native --no-default-features
```

**Các tính năng có sẵn:**

| Tính năng            | Mô tả                                                           |
| -------------------- | --------------------------------------------------------------- |
| `mimalloc`           | Sử dụng bộ cấp phát mimalloc (được bật theo mặc định)           |
| `deadlock_detection` | Bật tính năng phát hiện bế tắc (deadlock) của parking_lot để gỡ lỗi sự cố khóa |
| `dhat-heap`          | Bật cấu hình heap (heap profiling) bằng dhat                    |

**Phát hiện Bế tắc (Deadlock Detection)** đặc biệt hữu ích trong quá trình phát triển nếu bạn gặp tình trạng treo hoặc nghi ngờ có sự cố liên quan đến khóa. Khi được bật, parking_lot sẽ phát hiện các bế tắc tiềm ẩn và kích hoạt hoảng loạn (panic) với thông tin chẩn đoán.

### Chạy máy chủ

```bash
# Run directly with cargo (debug mode)
cargo run-native

# Run with release optimizations
cargo run-native --release

# Or run the built binary directly
./target/release/steel
```

## Các bước tiếp theo

Bây giờ bạn đã chạy được Steel, hãy tiến tới phần [Cấu hình](../../configuration/overview) để tùy chỉnh máy chủ của bạn.
