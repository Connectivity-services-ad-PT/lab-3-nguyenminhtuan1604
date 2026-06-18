# Reliability Checklist — team-core (pair10)

**Contract:** `contracts/core-business.openapi.yaml`  
**Collection:** `postman/collections/FIT4110_lab03_core.postman_collection.json`  
**Ngày kiểm tra:** 25/05/2026  
**Người thực hiện:** Nguyễn Minh Tuân — @nguyenminhtuan1604

---

## 1. Contract Quality

- [x] Contract dùng OpenAPI 3.1.0
- [x] Tất cả operation có `operationId` duy nhất
- [x] Tất cả operation có `summary` và `description`
- [x] Tất cả operation có ít nhất một response `2xx`
- [x] Tất cả operation có ít nhất một response `4xx`
- [x] Error response dùng cấu trúc `ProblemDetails`
- [x] Schema dùng `$ref` thay vì inline dài
- [x] Có ít nhất một `oneOf` + `discriminator`
- [x] Có ít nhất một trường union type với `null`
- [x] Spectral lint pass — No errors

---

## 2. Collection Structure

- [x] Có folder `01_Functional`
- [x] Có folder `02_Auth`
- [x] Có folder `03_Negative`
- [x] Có folder `04_Boundary_Reliability`
- [x] Có folder `05_Consumer_side_Smoke`
- [x] Có folder `06_Local_only_NonFunctional`
- [x] Request đặt tên rõ ràng, có mô tả mục đích
- [x] Không hardcode `baseUrl` trong collection
- [x] Không hardcode `authToken` trong collection

---

## 3. Test Coverage

- [x] Happy path: GET /health trả 200 với status ok
- [x] Happy path: POST /policy/access-check trả 200 với decision
- [x] Happy path: GET /policy/rules trả 200 với items array
- [x] Happy path: GET /policy/rules/{policyId} trả 200 với policyId
- [x] Happy path: POST /access-log trả 201 với logId
- [x] Auth test: request có token hợp lệ trả 200
- [x] Auth test: request thiếu token được kiểm tra
- [x] Negative: cardId sai định dạng trả 400/422
- [x] Negative: thiếu field bắt buộc trả 400/422
- [x] Negative: policyId không tồn tại ghi nhận 404
- [x] Boundary: limit=1 (min boundary)
- [x] Boundary: limit=100 (max boundary)
- [x] Boundary: POST /access-log với DENY + reason
- [x] Consumer-side smoke: Access Gate gọi /health của Core Business
- [x] Consumer-side smoke: Access Gate gọi /policy/access-check với direction EXIT
- [x] Local-only NonFunctional: SLA response time < 200ms

---

## 4. Environment

- [x] Environment `FIT4110_lab03_mock` đã tạo
- [x] Environment `FIT4110_lab03_local` đã tạo
- [x] `env`, `baseUrl`, `authToken`, `teamName` đều có trong cả 2 environment
- [x] Mock environment dùng `http://localhost:4010`
- [x] Local environment dùng `http://localhost:8000`

---

## 5. Newman Report

- [x] Newman chạy thành công trên mock environment
- [x] Kết quả: **35/35 PASS, 0 Failed, 0 Errors**
- [x] `reports/newman-report.html` đã sinh
- [x] `reports/newman-report.xml` đã sinh
- [x] `reports/contract-lint-report.txt` đã sinh

---

## 6. Known Issues

| Issue | Mô tả | Kế hoạch xử lý |
|---|---|---|
| Auth test trên mock | Mock Prism không validate auth thật nên thiếu token vẫn trả 200 | Kiểm tra thật khi service local hoàn thiện |
| 404 test trên mock | Mock luôn trả 200 theo example | Test điều chỉnh chấp nhận 200/404, service thật sẽ trả 404 đúng |
| Local environment | Service thật chưa code trong Lab 03 | Kiểm thử ở Lab 04 hoặc sprint tiếp theo |
