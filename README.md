# 🌍 Time.Now World Time API

The **Time.Now API** is a free, high-performance JSON interface for developers. It provides current local time, atomic synchronization data, timezone offsets, daylight saving (DST) rules, and IP geolocation.

- **Base URL:** `https://time.now/developer/api`
- **Authentication:** None required
- **Cost:** Free for commercial and personal use
- **Attribution:** Please link back to [Time.Now](https://time.now) in your application

---

## 🚀 Features

- **RESTful JSON API** – Simple, predictable responses
- **Timezone Aware** – Accurate IANA timezone handling (e.g., `Europe/London`)
- **Geolocation** – Automatic IP-to-Timezone lookup
- **DST Logic** – Automatically calculates Daylight Saving Time offsets and status
- **CORS Enabled** – Ready for frontend usage (React, Vue, etc.)

---

## 🔌 API Endpoints

### 1. Get Time by Timezone

Returns the current time object for a specific IANA timezone.

**Request**

```http
GET /timezone/:area/:location
```

**Example**

```bash
curl https://time.now/developer/api/timezone/Europe/London
```

**JSON Response**

```json
{
  "abbreviation": "BST",
  "datetime": "2023-10-05T14:30:00.123456+01:00",
  "day_of_week": 4,
  "day_of_year": 278,
  "dst": true,
  "dst_offset": 3600,
  "timezone": "Europe/London",
  "unixtime": 1696512600,
  "utc_offset": "+01:00",
  "week_number": 40
}
```

---

### 2. Get Time by IP (Auto-Detect)

Detects the public IP address of the requester and returns the local time.  
Useful for auto-detecting a user's local time without requesting location permissions.

**Request**

```http
GET /ip
```

---

### 3. List All Timezones

Returns a list of all valid IANA timezone strings as a JSON array.

**Request**

```http
GET /timezone
```

---

## 💻 Integration Examples

### JavaScript (Frontend / Fetch)

Check time on the client side without relying on the user's potentially incorrect system clock.

```javascript
fetch('https://time.now/developer/api/ip')
  .then(response => response.json())
  .then(data => {
    console.log('Current time:', data.datetime);
    console.log('Timezone:', data.timezone);
  });
```

---

### Python (Backend)

Ideal for server logs, data synchronization, or IoT devices.

```python
import requests

def get_server_time(timezone="UTC"):
    url = f"https://time.now/developer/api/timezone/{timezone}"
    try:
        response = requests.get(url)
        response.raise_for_status()
        data = response.json()
        print(f"Time in {timezone}: {data['datetime']}")
    except Exception as e:
        print(f"Error: {e}")

get_server_time("Asia/Tokyo")
```

---

### PHP (WordPress / Legacy)

```php
<?php
$timezone = "Europe/Paris";
$json = file_get_contents("https://time.now/developer/api/timezone/" . $timezone);
$data = json_decode($json, true);

if ($data) {
    echo "Current time: " . $data['datetime'];
}
?>
```

---

### Bash (Shell / cURL)

Great for system administrators and cron jobs.

```bash
# Get ISO time string for logs
curl -s https://time.now/developer/api/timezone/UTC | jq -r '.datetime'
```

---

## ❤️ Terms of Service

This API is provided completely free of charge.  
In exchange, we kindly ask that you provide a link back to Time.Now on your website footer, README, or application **About** screen.

```html
<a href="https://time.now">World Time API by Time.Now</a>
```
