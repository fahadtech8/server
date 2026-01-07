# Multi-Platform Remote License & Expiry System

Yeh ek advanced security system hai jo aapki websites (PHP, Laravel, WordPress, aur HTML Landing Pages) ko ek central GitHub JSON file ke zariye control karta hai. Isme **Multiple License Support** aur **Time-Based Expiry** features shamil hain.

---

## 1. GitHub Configuration (`controller.json`)

Sabse pehle apni GitHub repository mein `controller.json` file banayein. Yeh aapka main control panel hai.

**Format:**
```json
{
  "licenses": {
    "MY-SECURE": "2026-12-31",
    "FAHAD-786": "2025-06-01",
    "WP-KEY-99": "2025-01-15"
  },
  "title": "License Alert",
  "msg_expired": "Aapka license expire ho gaya hai. Please renew karein.",
  "msg_invalid": "Invalid License! Please admin se contact karein."
}
