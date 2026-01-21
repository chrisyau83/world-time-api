# 📚 World Time API Integration Cookbook

A collection of code snippets to help you integrate the [Time.Now API](https://time.now/developer) into your application, regardless of the language or platform.

---

## Table of Contents

- [Backend & Scripting](#backend--scripting)
- [Frontend & Mobile](#frontend--mobile)
- [IoT & Hardware](#iot--hardware)
- [System Admin](#system-admin)
- [Game Development](#game-development)

---

## Backend & Scripting

### Python (Requests)
Ideal for data synchronization scripts.

```python
import requests

def get_current_time(timezone="UTC"):
    url = f"https://time.now/developer/api/timezone/{timezone}"
    try:
        response = requests.get(url)
        response.raise_for_status()
        data = response.json()
        print(f"Time in {timezone}: {data['datetime']}")
        print(f"DST Active: {data['dst']}")
    except requests.exceptions.RequestException as e:
        print(f"Error fetching time: {e}")

# Example: Get Tokyo Time
get_current_time("Asia/Tokyo")
```

---

### Node.js (Axios)
Perfect for serverless functions (AWS Lambda, Vercel).

```javascript
const axios = require('axios');

async function getServerTime() {
  try {
    const response = await axios.get(
      'https://time.now/developer/api/timezone/America/Los_Angeles'
    );
    const { datetime, utc_offset, dst } = response.data;

    console.log(`LA Time: ${datetime}`);
    console.log(`Offset: ${utc_offset}`);
    console.log(`DST Active: ${dst}`);
  } catch (error) {
    console.error('Time fetch failed:', error.message);
  }
}

getServerTime();
```

---

### PHP
Zero external dependencies.

```php
<?php
$timezone = "Europe/London";
$url = "https://time.now/developer/api/timezone/" . $timezone;

$json = file_get_contents($url);
$data = json_decode($json, true);

if ($data) {
    echo "Current time in " . $data['timezone'] . ": " . $data['datetime'];
}
?>
```

---

### Go (Golang)
Strictly typed struct example.

```go
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
)

type TimeResponse struct {
    Datetime string `json:"datetime"`
    Timezone string `json:"timezone"`
    UnixTime int64  `json:"unixtime"`
}

func main() {
    resp, err := http.Get("https://time.now/developer/api/timezone/Europe/Berlin")
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()

    var record TimeResponse
    if err := json.NewDecoder(resp.Body).Decode(&record); err != nil {
        panic(err)
    }

    fmt.Printf("Time in %s is %s\n", record.Timezone, record.Datetime)
}
```

---

### Java (HttpClient)
Works with Java 11+.

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class WorldTime {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://time.now/developer/api/timezone/Asia/Singapore"))
                .build();

        client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
                .thenApply(HttpResponse::body)
                .thenAccept(System.out::println)
                .join();
    }
}
```

---

## Frontend & Mobile

### React Hook
Custom hook for hydration-safe time and geolocation.

```javascript
import { useState, useEffect } from 'react';

const useServerTime = () => {
  const [timeData, setTimeData] = useState(null);

  useEffect(() => {
    fetch('https://time.now/developer/api/ip')
      .then(res => res.json())
      .then(data => setTimeData(data))
      .catch(err => console.error(err));
  }, []);

  return timeData;
};

export default useServerTime;
```

---

### iOS (Swift)
Prevent users from cheating by changing device time.

```swift
import Foundation

struct TimeResponse: Codable {
    let datetime: String
    let timezone: String
}

func fetchTime() {
    let url = URL(string: "https://time.now/developer/api/timezone/Europe/Paris")!

    URLSession.shared.dataTask(with: url) { data, _, _ in
        if let data = data {
            if let response = try? JSONDecoder().decode(TimeResponse.self, from: data) {
                print("Paris Time: \(response.datetime)")
            }
        }
    }.resume()
}
```

---

## IoT & Hardware

### Arduino / ESP32
Get atomic time without complex NTP libraries.

```cpp
#include <HTTPClient.h>
#include <ArduinoJson.h>

void getTime() {
  HTTPClient http;
  http.begin("https://time.now/developer/api/timezone/Europe/Paris");
  int httpCode = http.GET();

  if (httpCode > 0) {
    String payload = http.getString();

    StaticJsonDocument<512> doc;
    deserializeJson(doc, payload);

    const char* datetime = doc["datetime"];
    Serial.println(datetime);
  }
  http.end();
}
```

---

## System Admin

### PowerShell
Windows administration and logging.

```powershell
$url = "https://time.now/developer/api/timezone/Asia/Tokyo"
$response = Invoke-RestMethod -Uri $url

Write-Host "Current Time in Tokyo:" $response.datetime
Write-Host "Is DST Active:" $response.dst
```

---

## Game Development

### Unity (C#)
Prevent time-skipping cheats in mobile games.

```csharp
using UnityEngine;
using UnityEngine.Networking;
using System.Collections;

public class TimeChecker : MonoBehaviour {
    void Start() {
        StartCoroutine(GetTrustedTime());
    }

    IEnumerator GetTrustedTime() {
        string url = "https://time.now/developer/api/timezone/UTC";
        using (UnityWebRequest webRequest = UnityWebRequest.Get(url)) {
            yield return webRequest.SendWebRequest();

            if (webRequest.result == UnityWebRequest.Result.Success) {
                Debug.Log("Server Time JSON: " + webRequest.downloadHandler.text);
            }
        }
    }
}
```

