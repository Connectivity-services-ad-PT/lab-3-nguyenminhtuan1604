# Consumer-Provider Handshake — pair10

## Thông tin cặp

| | Consumer | Provider |
|---|---|---|
| **Service** | Access Gate Service | Core Business Service |
| **Nhóm** | Nhóm Access Gate | Nguyễn Minh Tuân |
| **Contract** | `contracts/core-business.openapi.yaml` | `contracts/core-business.openapi.yaml` |

---

## Cam kết của Provider (Core Business)

Provider cam kết cung cấp các endpoint sau theo đúng contract v1.0:

| Endpoint | Method | Mục đích | SLA |
|---|---|---|---|
| `/health` | GET | Kiểm tra service còn sống | < 200ms |
| `/policy/access-check` | POST | Kiểm tra policy ra/vào realtime | < 200ms |
| `/policy/rules` | GET | Lấy danh sách policy rules | < 500ms |
| `/policy/rules/{policyId}` | GET | Lấy chi tiết một policy rule | < 500ms |
| `/access-log` | POST | Nhận log quẹt thẻ từ Access Gate | < 500ms |

Provider cam kết:
- Response format theo đúng schema trong `openapi.yaml`
- Error response dùng `ProblemDetails` (RFC 7807)
- Bearer token authentication bắt buộc (trừ `/health`)
- Mock server luôn sẵn sàng tại `http://localhost:4010` cho Consumer test sớm

---

## Cam kết của Consumer (Access Gate)

Consumer cam kết:
- Gọi đúng endpoint và method theo contract
- Gửi đúng schema request body (cardId, gateId, direction, timestamp)
- Luôn gửi Bearer token trong Authorization header
- Gửi log lên `/access-log` sau mỗi lần quyết định ALLOW/DENY
- Cache policy rules tối đa 5 phút, refresh sau đó

---

## Consumer-side Smoke Test

Consumer đã viết smoke test gọi mock của Core Business:

| Test | Endpoint | Kết quả |
|---|---|---|
| Core Business còn sống | GET /health | ✅ PASS |
| Kiểm tra policy ENTER | POST /policy/access-check | ✅ PASS |
| Kiểm tra policy EXIT | POST /policy/access-check (direction=EXIT) | ✅ PASS |

---

## Sign-off

| Vai trò | Tên | Ngày | Chữ ký |
|---|---|---|---|
| Provider | Nguyễn Minh Tuân — Core Business | 25/05/2026 | Nguyễn Minh Tuân |
| Consumer | Nhóm Access Gate | 25/05/2026 | _(Consumer ký)_ |
| Witness | FIT4110 Teaching Team | 25/05/2026 | _(GV/TA ký)_ |

---

## Ghi chú

- Contract version: v1.0.0
- Mock server: Prism 5.15.10
- Newman: 6.2.2
- Spectral lint: No errors