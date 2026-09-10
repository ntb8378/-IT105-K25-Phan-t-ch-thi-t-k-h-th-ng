KHỬ QUAN HỆ NHIỀU-NHIỀU BẰNG ASSOCIATION CLASS - RIKKEILEARN
1. Phân tích bài toán & Thiết lập Bội số (Multiplicity)
Vấn đề của thiết kế cũ:
Quan hệ nhiều-nhiều trực tiếp giữa Student (*) và Course (*) không có nơi lưu trữ thông tin phát sinh từ mối liên kết này (cụ thể là enrollDate).
Giải pháp với Lớp trung gian (Association Class):
Thêm lớp Enrollment nằm giữa Student và Course với thuộc tính - enrollDate: Date.
Phân rã đường nối * -- * thành 2 đường nối với bội số chính xác:
Student (1) -- (0..*) Enrollment: Một học viên có thể chưa đăng ký khóa nào hoặc đăng ký nhiều khóa (0..*). Mỗi bản ghi đăng ký thuộc về duy nhất một học viên (1).
Course (1) -- (0..*) Enrollment: Một khóa học mới mở có thể chưa có ai đăng ký hoặc có nhiều học viên (0..*). Mỗi bản ghi đăng ký gắn liền với duy nhất một khóa học (1).
2. Sơ đồ Class Diagram
