## Cấu hình

Workflow `.github/workflows/ci.yml` sử dụng `actions/setup-java@v5` với cấu hình:

```yaml
with:
  java-version: '17'
  distribution: 'temurin'
  cache: 'gradle'
```

Cache key được tạo từ các tệp cấu hình Gradle. Ở lần chạy đầu, dependency được tải và lưu vào cache; lần chạy tiếp theo có cùng cache key sẽ khôi phục cache này.

## Kết quả thực nghiệm

> Điền số liệu hiển thị trong trang **GitHub Actions** sau hai lần push liên tiếp.

| Lần chạy | Trạng thái cache trong `setup-java` | Thời gian `setup-java` | Thời gian `./gradlew test` | Tổng thời gian job |
|---|---|---:|---:|---:|
| Push 1 | Cache miss / tạo cache mới | Chưa đo | Chưa đo | Chưa đo |
| Push 2 | `Cache restored successfully from key: gradle-...` | Chưa đo | Chưa đo | Chưa đo |

## Nhận xét

Sau khi thực hiện hai lần push, thay các ô **Chưa đo** bằng thời gian thực tế. Lần chạy thứ hai đạt yêu cầu khi log của bước `setup-java` có thông báo khôi phục Gradle cache thành công. So sánh tổng thời gian job và thời gian bước `./gradlew test` để đánh giá mức cải thiện.