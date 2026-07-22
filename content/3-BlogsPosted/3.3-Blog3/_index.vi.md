---
title: "Blog 3"
date: 2026-07-10
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---


# Xây dựng hệ thống xác thực hai lớp cho Nakama Game Server với Amazon Cognito trên AWS

Trong game trực tuyến, xác thực phải vừa an toàn vừa mượt mà. Nakama sử dụng **Session Token** quản lý phiên chơi, còn **Amazon Cognito** phát hành **JWT** để xác thực danh tính người dùng. Giải pháp xác thực hai lớp (Dual-Token Authentication) tách biệt danh tính và phiên chơi, giúp trải nghiệm đăng nhập liền lạc hơn.

Bài viết này mô tả kiến trúc kết hợp Amazon Cognito, CloudFront, ALB, NLB, ECS Fargate và Nakama cùng **Go Runtime Hook** để xác thực JWT trước khi tạo Session Token.

---

## Tại sao cần xác thực hai lớp?

Một hệ thống game cần giải quyết hai vấn đề riêng biệt:

- **Xác thực người dùng** (ai là người chơi)
- **Quản lý phiên chơi** (phiên chơi hợp lệ và có quyền)

Nếu Nakama và Cognito hoạt động độc lập, người chơi có thể phải đăng nhập lại nhiều lần hoặc gặp gián đoạn khi token hết hạn. Xác thực hai lớp cho phép Cognito chịu trách nhiệm danh tính, còn Nakama chịu trách nhiệm phiên chơi.

---

## Tổng quan kiến trúc

Giải pháp bao gồm:

- **Amazon Cognito** xác thực người dùng và cấp JWT
- **Amazon CloudFront** là điểm truy cập HTTPS công khai
- **AWS WAF** lọc yêu cầu độc hại trước khi vào hệ thống
- **Application Load Balancer (ALB)** xử lý HTTP/HTTPS và OIDC
- **Network Load Balancer (NLB)** xử lý lưu lượng Nakama thời gian thực và WebSocket
- **Amazon ECS Fargate** chạy Nakama
- **Go Runtime Hook** trong Nakama xác minh JWT Cognito rồi tạo Session Token

Kiến trúc này tách biệt rõ ràng việc xác thực danh tính, từ đó tối ưu bảo mật và khả năng mở rộng.

---

## Luồng xác thực

1. Người chơi đăng nhập với Cognito bằng **USER_SRP_AUTH**.
2. Cognito trả về JWT chứa claim **sub**, **iss**, **exp**, **client_id**.
3. Client gửi JWT đến Nakama khi yêu cầu truy cập game.
4. Go Runtime Hook của Nakama kiểm tra chữ ký, issuer, audience, thời hạn và client ID.
5. Nếu token hợp lệ, Nakama tạo **Session Token** riêng cho phiên chơi.

Nhờ vậy, hệ thống phân tách rõ ràng giữa việc đăng nhập và việc tạo phiên chơi.

---

## Kiểm tra JWT trong Go Runtime Hook

Các bước kiểm tra chính trong Hook:

- xác minh chữ ký JWT bằng public keys của Cognito
- xác nhận **iss** khớp issuer của user pool
- xác nhận **aud** / **client_id** đúng ứng dụng game
- xác nhận token chưa hết hạn
- lấy **sub** làm định danh người chơi chính thức

Khi JWT hợp lệ, Nakama gán **sub** vào bản ghi người dùng nội bộ và tạo Session Token phù hợp.

---

## Vai trò của ALB và NLB

Thiết kế này dùng hai Load Balancer để xử lý các loại lưu lượng khác nhau:

- **ALB** xử lý traffic HTTP/HTTPS điều khiển, callback xác thực và REST API.
- **NLB** xử lý kết nối Nakama thời gian thực, WebSocket và các kết nối thấp độ trễ.

Dùng NLB cho Nakama giúp duy trì hiệu năng thời gian thực trong khi ALB đảm bảo HTTPS và OIDC.

---

## Bảo mật hệ thống

Bảo mật được thiết lập theo lớp:

- **CloudFront + AWS WAF** là điểm vào công khai, chặn yêu cầu độc hại.
- **Security Group** giới hạn lưu lượng chỉ cho phép CloudFront và load balancer truy cập Nakama.
- **Xác minh JWT Cognito** ngăn chặn replay và giả mạo.
- Go Runtime Hook dùng **sub** từ JWT đã xác thực làm danh tính người chơi xác thực.

Cách tiếp cận này giảm diện phơi bày và tập trung các quyết định xác thực.

---

## Lợi ích cho nền tảng game

- tách biệt danh tính và quản lý phiên chơi
- hỗ trợ single sign-on với Cognito nhưng vẫn giữ quyền kiểm soát phiên Nakama
- tối ưu cho WebSocket và traffic thời gian thực qua NLB
- tập trung xác thực trong Go Runtime Hook
- cho phép Nakama quản lý vòng đời phiên tùy theo game, độc lập với thời hạn JWT Cognito

---

## Kết luận

Xác thực hai lớp cho Nakama trên AWS là một mô hình an toàn và phù hợp cho game trực tuyến. Cognito chịu trách nhiệm đăng nhập và phát JWT, Nakama quản lý phiên chơi bằng Session Token riêng. Kết hợp với CloudFront, ALB, NLB, ECS Fargate và Go Runtime Hook, giải pháp này vừa bảo mật vừa mang lại trải nghiệm người chơi ổn định.

---

## Tham khảo

- AWS Architecture Blog: Dual-Token Authentication for Nakama Game Servers with Amazon Cognito on AWS
