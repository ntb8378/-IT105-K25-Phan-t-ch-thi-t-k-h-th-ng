THIẾT KẾ ĐÓNG GÓI LỚP HỌC VIÊN (STUDENT) - RIKKEILEARN
Phần 1 - Sơ đồ lớp (Class Diagram với PlantUML)
Sử dụng PlantUML để biểu diễn sơ đồ lớp với các ký hiệu bổ từ truy cập chuẩn UML:

- : Private (thuộc tính nội bộ, không cho phép truy cập trực tiếp từ bên ngoài)
+ : Public (phương thức công khai làm giao diện tương tác an toàn)


Phần 2 - Mô tả logic & Mã giả (Pseudocode)
Mã giả kiểm tra ràng buộc dữ liệu:
FUNCTION setScore(score: float) -> void:
    IF score >= 0.0 AND score <= 10.0 THEN
        SET this.averageScore = score
    ELSE
        RAISE ValueError("Điểm trung bình không hợp lệ! Điểm phải nằm trong dải [0, 10].")
    END IF
END FUNCTION