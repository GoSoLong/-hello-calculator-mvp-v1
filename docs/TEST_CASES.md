# Test cases - Hello Calculator MVP v1.1

| ID | Thao tác | Kết quả mong đợi |
|---|---|---|
| T01 | `2 + 3 =` | `5` |
| T02 | `9 − 4 =` | `5` |
| T03 | `6 × 7 =` | `42` |
| T04 | `20 ÷ 5 =` | `4` |
| T05 | `1 . 5 + 2 =` | `3.5` |
| T06 | `10 ÷ 0 =` | `Error` |
| T07 | `1 . 2 . 3` | `1.23` |
| T08 | `123 ⌫` | `12` |
| T09 | `123 C` | `0` |
| T10 | `12 + − 5 =` | operator cuối cùng là `−`, kết quả `7` |
| T11 | `5 + 3 =`, sau đó `2` | Display trở thành `2` |
| T12 | Bàn phím `8 * 9 Enter` | `72` |
| T13 | Bàn phím `Escape` | reset về `0` |
| T14 | Viewport 320 px | không có horizontal page scroll |
| T15 | Tab qua keypad | focus-visible rõ ràng |
| T16 | `8 ±` | `-8` |
| T17 | `8 ± ±` | `8` |
| T18 | `25 %` | `0.25` |
| T19 | `5 + 2 ± =` | `3` |
| T20 | `F9`, sau đó `6` | `-6` |
| T21 | `12`, sau đó nhấn `Shift+5` | `0.12` |
| T22 | `5 + ± 6 =` | `-1` |

## Acceptance checklist

- [ ] Tất cả test T01–T22 đạt.
- [ ] Không có lỗi JavaScript trong DevTools Console.
- [ ] Learning Panel cập nhật Event/Value/State đúng.
- [ ] Chrome/Edge/Firefox hiện đại hiển thị ổn định.
- [ ] GitHub Pages mở được bằng URL HTTPS.
