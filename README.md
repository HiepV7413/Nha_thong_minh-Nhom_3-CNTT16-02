# 🏠 AI & IoT Nhà Thông Minh

<div align="center">
  <img src="docs/images/smart_home_logo.png" alt="Smart Home Logo" width="200"/>
  <!-- Bạn có thể thay thế logo và hình ảnh phù hợp -->
</div>

<h3 align="center">Hệ Thống Nhà Thông Minh Tích Hợp AI & IoT</h3>

<p align="center">
  <strong>Giải pháp giám sát và điều khiển thiết bị thông qua cảm biến và công nghệ kết nối không dây</strong>
</p>

<p align="center">
  <a href="#-kiến-trúc">Kiến trúc</a> •
  <a href="#-tính-năng-chính">Tính năng</a> •
  <a href="#-các-module">Các Module</a> •
  <a href="#-cài-đặt">Cài đặt</a> •
  <a href="#-hướng-dẫn-sử-dụng">Hướng dẫn sử dụng</a> •
  <a href="#-flask-server--web-html">Flask Server & Web HTML</a> •
  <a href="#-tài-liệu">Tài liệu</a>
</p>

## 🏗️ Kiến trúc

Hệ thống được xây dựng theo mô hình phân tầng gồm:

1. **Lớp Cảm Biến & Thiết Bị (Edge Devices)**  
   Các module (Phòng Bếp, Phòng Khách, Phòng Ngủ, Cửa Ra Vào, …) sử dụng ESP32/ESP8266 kết nối với các cảm biến như DHT11, cảm biến khí gas, HC-SR04, cảm biến mưa, RFID,… và các thiết bị điều khiển như buzzer, quạt, LED, servo,…

2. **Lớp Giao Tiếp & Xử Lý Dữ Liệu**  
   Các module gửi dữ liệu qua WiFi đến Server Flask thông qua HTTP request để cập nhật trạng thái và nhận lệnh điều khiển.

3. **Lớp Server & Giao Diện Web**  
   Server Flask tiếp nhận và xử lý dữ liệu từ các module, đồng thời cung cấp API cho việc điều khiển thiết bị. Giao diện Web HTML hiển thị trạng thái cảm biến và cho phép điều khiển trực tiếp.

<p align="center">
  <img src="docs/images/architecture_smart_home.png" alt="Kiến trúc hệ thống" width="800"/>
</p>

## ✨ Tính năng chính

- **Giám sát môi trường đa phòng:**  
  - **Phòng Bếp:** Theo dõi nhiệt độ, độ ẩm, mức khí gas; kích hoạt báo động (buzzer, quạt, LED báo động) nếu vượt ngưỡng an toàn.
  - **Phòng Khách:** Đo nhiệt độ, độ ẩm và phát hiện chuyển động (HC-SR04); điều khiển LED, quạt và cảm biến.
  - **Phòng Ngủ:** Giám sát nhiệt độ, độ ẩm, trạng thái mưa; điều khiển đèn, quạt và cửa sổ tự động (servo) – tự động đóng cửa sổ khi mưa.
  - **Cửa Ra Vào:** Xác thực truy cập qua thẻ RFID và bàn phím; mở/đóng cửa thông qua điều khiển servo và hiển thị thông báo trên LCD.

- **Điều khiển thiết bị từ xa:**  
  Đồng bộ trạng thái của các thiết bị (LED, quạt, cửa sổ, cảm biến) qua server và giao diện web.

- **Giao tiếp qua WiFi & HTTP:**  
  Mỗi module gửi và nhận dữ liệu từ server Flask theo định dạng JSON và qua các endpoint API.

- **Giao diện Web:**  
  Hiển thị dữ liệu cảm biến cập nhật theo thời gian thực và cung cấp các nút điều khiển cho từng phòng.

## 📂 Các Module

### 1. Phòng Bếp

- **Chức năng:**  
  - Đo nhiệt độ, độ ẩm (DHT11) và mức khí gas.  
  - Hiển thị thông tin lên màn hình LCD I2C.  
  - Kích hoạt báo động (buzzer, quạt, LED báo động) khi nhiệt độ hoặc mức khí gas vượt ngưỡng.  
  - Đồng bộ trạng thái LED với server.

- **Thư viện sử dụng:**  
  - `WiFi.h`, `HTTPClient.h`  
  - `LiquidCrystal_I2C.h`  
  - `DHT.h`

- **Đoạn mã mẫu:**  
  ```cpp
  // Khởi tạo WiFi, cảm biến DHT11, LCD I2C và các thiết bị như buzzer, quạt, LED.
  // Hàm gửi dữ liệu cảm biến và cập nhật trạng thái từ server.
  // ... (xem chi tiết trong code Phòng Bếp)
  ```

### 2. Phòng Khách

- **Chức năng:**  
  - Đo nhiệt độ, độ ẩm (DHT11) và phát hiện chuyển động qua cảm biến khoảng cách HC-SR04.  
  - Điều khiển LED và quạt theo lệnh từ server.  
  - Cập nhật trạng thái cảm biến (bật/tắt) theo yêu cầu.

- **Thư viện sử dụng:**  
  - `ESP8266WiFi.h`, `ESP8266HTTPClient.h`  
  - `DHT.h`

- **Đoạn mã mẫu:**  
  ```cpp
  // Cấu hình WiFi, cảm biến DHT11 và HC-SR04 của Phòng Khách.
  // Hàm gửi dữ liệu cảm biến, cập nhật trạng thái LED, quạt và cảm biến từ server.
  // ... (xem chi tiết trong code Phòng Khách)
  ```

### 3. Phòng Ngủ

- **Chức năng:**  
  - Đo nhiệt độ, độ ẩm (DHT11) và theo dõi tình trạng mưa (Rain sensor).  
  - Điều khiển đèn LED, quạt và cửa sổ tự động (servo) dựa trên dữ liệu cảm biến và lệnh từ server.
  - Tự động đóng cửa sổ khi phát hiện mưa.

- **Thư viện sử dụng:**  
  - `ESP8266WiFi.h`, `ESP8266HTTPClient.h`  
  - `DHT.h`, `Servo.h`

- **Đoạn mã mẫu:**  
  ```cpp
  // Cấu hình WiFi, DHT11, cảm biến mưa và servo điều khiển cửa sổ cho Phòng Ngủ.
  // Hàm gửi dữ liệu cảm biến và cập nhật trạng thái từ server.
  // ... (xem chi tiết trong code Phòng Ngủ)
  ```

### 4. Cửa Ra Vào

- **Chức năng:**  
  - Xác thực truy cập bằng thẻ RFID và bàn phím ma trận.  
  - So sánh UID của thẻ với danh sách UID hợp lệ để mở cửa.  
  - Hỗ trợ mở/đóng cửa qua việc nhập mật khẩu từ bàn phím.
  - Điều khiển servo mở/đóng cửa và hiển thị trạng thái trên LCD.

- **Thư viện sử dụng:**  
  - `Keypad.h` – xử lý bàn phím ma trận  
  - `Servo.h` – điều khiển servo mở/đóng cửa  
  - `LiquidCrystal_I2C.h` – hiển thị thông báo trên LCD  
  - `SPI.h` và `MFRC522.h` – giao tiếp với module RFID RC522

- **Đoạn mã mẫu:**  
  ```cpp
  // Khởi tạo LCD I2C, servo, module RFID RC522 và bàn phím ma trận.
  // Xác thực thẻ RFID qua UID và kiểm tra mật khẩu nhập vào để điều khiển cửa.
  // ... (xem chi tiết trong code Cửa Ra Vào)
  ```

## 📥 Cài đặt

### Yêu cầu phần cứng

- **Board điều khiển:** ESP32 hoặc ESP8266 (cho các module cảm biến)  
- **Cảm biến & Module:**  
  - DHT11 (cho Phòng Bếp, Phòng Khách, Phòng Ngủ)  
  - Cảm biến khí gas (Phòng Bếp)  
  - HC-SR04 (Phòng Khách)  
  - Cảm biến mưa (Phòng Ngủ)  
  - Module RFID RC522 và bàn phím ma trận (Cửa Ra Vào)
- **Thiết bị điều khiển:**  
  - LCD I2C (cho Phòng Bếp và Cửa Ra Vào)  
  - Buzzer, quạt, LED (cho các module)  
  - Servo (cho cửa sổ ở Phòng Ngủ và cửa ra vào)

### Yêu cầu phần mềm

- **Thư viện Arduino:**  
  - WiFi, HTTPClient, LiquidCrystal_I2C, DHT, Servo  
  - ESP8266WiFi, ESP8266HTTPClient (cho board ESP8266)  
  - Keypad, SPI, MFRC522 (cho module Cửa Ra Vào)
- **Python Flask Server:** (xem mã nguồn bên dưới)
- **Giao diện Web:** (HTML & JavaScript – xem mã nguồn bên dưới)

### Hướng dẫn cài đặt

1. **Cài đặt Arduino IDE** và cấu hình board ESP32/ESP8266.  
2. **Cài đặt các thư viện cần thiết** qua Library Manager hoặc tải trực tiếp.
3. **Cấu hình thông số WiFi và Server:**  
   Cập nhật SSID, mật khẩu, địa chỉ IP, cổng và các endpoint API trong từng module phù hợp với hệ thống của bạn.
4. **Nạp chương trình** lên các board điều khiển tương ứng.

## 🚀 Flask Server & Web HTML

### Flask Server

Mã nguồn Flask Server dưới đây tiếp nhận dữ liệu cập nhật từ các module và cung cấp API để điều khiển trạng thái thiết bị (LED, quạt, cửa sổ, cảm biến):

```python
from flask import Flask, request, render_template, jsonify

app = Flask(__name__)

# Lưu trữ dữ liệu cảm biến cho từng phòng
sensor_data = {
    "kitchen": {
        "temp": "N/A",
        "hum": "N/A",
        "gas": "N/A",
        "alarm": "SAFE",
        "led": "OFF"
    },
    "living_room": {
        "temp": "N/A",
        "hum": "N/A",
        "distance": "N/A",
        "motion": "NO",
        "led": "OFF",
        "fan": "OFF",
        "sensor": "YES"  # YES: cảm biến bật, NO: cảm biến tắt
    },
    "bedroom": {
        "temp": "N/A",
        "hum": "N/A",
        "rain": "DRY",    # "RAIN" nếu mưa, "DRY" nếu không mưa
        "led": "OFF",
        "fan": "OFF",
        "window": "CLOSED"  # "OPEN" hoặc "CLOSED"
    }
}

# Endpoint nhận dữ liệu cập nhật từ ESP32
@app.route('/update')
def update():
    room = request.args.get('room', 'kitchen')
    if room == "kitchen":
        temp = request.args.get('temp')
        hum = request.args.get('hum')
        gas = request.args.get('gas')
        alarm = request.args.get('alarm')
        if temp and hum and gas and alarm:
            sensor_data["kitchen"].update({
                "temp": temp,
                "hum": hum,
                "gas": gas,
                "alarm": alarm
            })
            return "Kitchen Data Updated", 200
        return "Missing parameters", 400
    elif room == "living_room":
        temp = request.args.get('temp')
        hum = request.args.get('hum')
        distance = request.args.get('distance')
        motion = request.args.get('motion')
        if temp and hum and distance and motion:
            led = sensor_data["living_room"].get("led", "OFF")
            fan = sensor_data["living_room"].get("fan", "OFF")
            sensor = sensor_data["living_room"].get("sensor", "YES")
            sensor_data["living_room"].update({
                "temp": temp,
                "hum": hum,
                "distance": distance,
                "motion": motion,
                "led": led,
                "fan": fan,
                "sensor": sensor
            })
            return "Living Room Data Updated", 200
        return "Missing parameters", 400
    elif room == "bedroom":
        temp = request.args.get('temp')
        hum = request.args.get('hum')
        rain = request.args.get('rain')
        if temp and hum and rain:
            led = sensor_data["bedroom"].get("led", "OFF")
            fan = sensor_data["bedroom"].get("fan", "OFF")
            window = sensor_data["bedroom"].get("window", "CLOSED")
            sensor_data["bedroom"].update({
                "temp": temp,
                "hum": hum,
                "rain": rain,
                "led": led,
                "fan": fan,
                "window": window
            })
            return "Bedroom Data Updated", 200
        return "Missing parameters", 400

# Endpoint cập nhật trạng thái LED
@app.route('/set_led')
def set_led():
    room = request.args.get('room', 'kitchen')
    state = request.args.get('state')
    if state:
        if room in sensor_data and "led" in sensor_data[room]:
            sensor_data[room]["led"] = "ON" if state.lower() == "on" else "OFF"
            return "LED state updated", 200
        return "Invalid room", 400
    return "Missing parameter", 400

# Endpoint cập nhật trạng thái Quạt (cho phòng khách & phòng ngủ)
@app.route('/set_fan')
def set_fan():
    room = request.args.get('room', 'living_room')
    state = request.args.get('state')
    if state:
        if room in sensor_data and "fan" in sensor_data[room]:
            sensor_data[room]["fan"] = "ON" if state.lower() == "on" else "OFF"
            return "Fan state updated", 200
        return "Invalid room", 400
    return "Missing parameter", 400

# Endpoint cập nhật trạng thái Cửa sổ (cho phòng ngủ)
@app.route('/set_window')
def set_window():
    room = request.args.get('room', 'bedroom')
    state = request.args.get('state')
    if state and room == "bedroom":
        sensor_data["bedroom"]["window"] = "OPEN" if state.lower() == "open" else "CLOSED"
        return "Window state updated", 200
    return "Missing parameter", 400

# Endpoint cập nhật trạng thái Cảm biến (cho phòng khách)
@app.route('/set_sensor')
def set_sensor():
    room = request.args.get('room', 'living_room')
    state = request.args.get('state')
    if state:
        if room in sensor_data and "sensor" in sensor_data[room]:
            sensor_data[room]["sensor"] = "YES" if state.lower() == "on" else "NO"
            return "Sensor state updated", 200
        return "Invalid room", 400
    return "Missing parameter", 400

# Endpoint trả về trạng thái LED
@app.route('/get_led')
def get_led():
    room = request.args.get('room', 'kitchen')
    if room in sensor_data and "led" in sensor_data[room]:
        return jsonify({"led": sensor_data[room]["led"]})
    return jsonify({"led": "N/A"})

# Endpoint trả về trạng thái Quạt
@app.route('/get_fan')
def get_fan():
    room = request.args.get('room', 'living_room')
    if room in sensor_data and "fan" in sensor_data[room]:
        return jsonify({"fan": sensor_data[room]["fan"]})
    return jsonify({"fan": "N/A"})

# Endpoint trả về trạng thái Cửa sổ
@app.route('/get_window')
def get_window():
    room = request.args.get('room', 'bedroom')
    if room in sensor_data and "window" in sensor_data[room]:
        return jsonify({"window": sensor_data[room]["window"]})
    return jsonify({"window": "N/A"})

# Endpoint trả về trạng thái Cảm biến
@app.route('/get_sensor')
def get_sensor():
    room = request.args.get('room', 'living_room')
    if room in sensor_data and "sensor" in sensor_data[room]:
        return jsonify({"sensor": sensor_data[room]["sensor"]})
    return jsonify({"sensor": "N/A"})

# Trang chính hiển thị dữ liệu cảm biến
@app.route('/')
def index():
    return render_template('index.html', sensor_data=sensor_data)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### Giao diện Web HTML

File `templates/index.html` dùng để hiển thị dữ liệu cảm biến và cung cấp nút điều khiển:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Quản lý Nhà Thông Minh</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f0f2f5;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 400px;
            margin: 2rem auto;
            background-color: #fff;
            padding: 1.5rem;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            margin-bottom: 2rem;
        }
        h1 {
            text-align: center;
            margin-bottom: 1rem;
        }
        p {
            font-size: 1rem;
            margin: 0.5rem 0;
        }
        button {
            display: inline-block;
            background-color: #007bff;
            color: #fff;
            padding: 0.6rem 1rem;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 1rem;
            margin-right: 0.5rem;
        }
        button:hover {
            background-color: #0056b3;
        }
        button[disabled] {
            opacity: 0.6;
            cursor: not-allowed;
        }
        .alert { color: #dc3545; font-weight: bold; }
        .safe { color: #28a745; font-weight: bold; }
    </style>
    <script>
      // PHÒNG BẾP
      function toggleKitchenLED(){
         var current = document.getElementById("kitchen_led").innerText.trim();
         var newState = (current.toLowerCase() === "off") ? "on" : "off";
         var xhr = new XMLHttpRequest();
         xhr.open("GET", "/set_led?room=kitchen&state=" + newState, true);
         xhr.onreadystatechange = function(){
             if(xhr.readyState==4 && xhr.status==200){
                 location.reload();
             }
         }
         xhr.send();
      }
      // PHÒNG KHÁCH
      function toggleLivingLED(){
         var current = document.getElementById("living_led").innerText.trim();
         var newState = (current.toLowerCase() === "off") ? "on" : "off";
         var xhr = new XMLHttpRequest();
         xhr.open("GET", "/set_led?room=living_room&state=" + newState, true);
         xhr.onreadystatechange = function(){
             if(xhr.readyState==4 && xhr.status==200){
                 location.reload();
             }
         }
         xhr.send();
      }
      function toggleLivingFan(){
         var current = document.getElementById("living_fan").innerText.trim();
         var newState = (current.toLowerCase() === "off") ? "on" : "off";
         var xhr = new XMLHttpRequest();
         xhr.open("GET", "/set_fan?room=living_room&state=" + newState, true);
         xhr.onreadystatechange = function(){
             if(xhr.readyState==4 && xhr.status==200){
                 location.reload();
             }
         }
         xhr.send();
      }
      function toggleLivingSensor(){
         var current = document.getElementById("living_sensor").innerText.trim();
         var newState = (current.toLowerCase() === "bật") ? "off" : "on";
         var xhr = new XMLHttpRequest();
         xhr.open("GET", "/set_sensor?room=living_room&state=" + newState, true);
         xhr.onreadystatechange = function(){
             if(xhr.readyState==4 && xhr.status==200){
                 location.reload();
             }
         }
         xhr.send();
      }
      // PHÒNG NGỦ
      function toggleBedroomLED(){
         var current = document.getElementById("bedroom_led").innerText.trim();
         var newState = (current.toLowerCase() === "off") ? "on" : "off";
         var xhr = new XMLHttpRequest();
         xhr.open("GET", "/set_led?room=bedroom&state=" + newState, true);
         xhr.onreadystatechange = function(){
             if(xhr.readyState==4 && xhr.status==200){
                 location.reload();
             }
         }
         xhr.send();
      }
      function toggleBedroomFan(){
         var current = document.getElementById("bedroom_fan").innerText.trim();
         var newState = (current.toLowerCase() === "off") ? "on" : "off";
         var xhr = new XMLHttpRequest();
         xhr.open("GET", "/set_fan?room=bedroom&state=" + newState, true);
         xhr.onreadystatechange = function(){
             if(xhr.readyState==4 && xhr.status==200){
                 location.reload();
             }
         }
         xhr.send();
      }
      function toggleBedroomWindow(){
         var current = document.getElementById("bedroom_window").innerText.trim();
         var newState = (current.toLowerCase() === "closed") ? "open" : "closed";
         var xhr = new XMLHttpRequest();
         xhr.open("GET", "/set_window?room=bedroom&state=" + newState, true);
         xhr.onreadystatechange = function(){
             if(xhr.readyState==4 && xhr.status==200){
                 location.reload();
             }
         }
         xhr.send();
      }
      // Tự động refresh trang mỗi 5 giây
      setTimeout(function(){
         window.location.reload();
      }, 5000);
    </script>
</head>
<body>
    <!-- PHÒNG BẾP -->
    <div class="container">
        <h1>Phòng Bếp</h1>
        <p>Nhiệt độ: {{ sensor_data.kitchen.temp }} °C</p>
        <p>Độ ẩm: {{ sensor_data.kitchen.hum }} %</p>
        <p>Nồng độ khí gas: {{ sensor_data.kitchen.gas }} ppm</p>
        <p>Tình trạng báo động: 
            {% if sensor_data.kitchen.alarm == "ALERT" %}
               <span class="alert">{{ sensor_data.kitchen.alarm }}</span>
            {% else %}
               <span class="safe">{{ sensor_data.kitchen.alarm }}</span>
            {% endif %}
        </p>
        <p>Đèn LED chính: <span id="kitchen_led">{{ sensor_data.kitchen.led }}</span></p>
        <button onclick="toggleKitchenLED()"
          {% if sensor_data.kitchen.alarm == "ALERT" %}
            disabled
          {% endif %}>
          {% if sensor_data.kitchen.alarm == "ALERT" %}
             KHÓA
          {% else %}
             {% if sensor_data.kitchen.led == "ON" %}
                TẮT ĐÈN
             {% else %}
                BẬT ĐÈN
             {% endif %}
          {% endif %}
        </button>
    </div>

    <!-- PHÒNG KHÁCH -->
    <div class="container">
        <h1>Phòng Khách</h1>
        <p>Nhiệt độ: {{ sensor_data.living_room.temp }} °C</p>
        <p>Độ ẩm: {{ sensor_data.living_room.hum }} %</p>
        <p>Khoảng cách: {{ sensor_data.living_room.distance }} cm</p>
        <p>Chuyển động: {{ sensor_data.living_room.motion }}</p>
        <p>Đèn LED: <span id="living_led">{{ sensor_data.living_room.led }}</span></p>
        <p>Quạt: <span id="living_fan">{{ sensor_data.living_room.fan }}</span></p>
        <p>Cảm biến HC-SR04: <span id="living_sensor">
            {% if sensor_data.living_room.sensor == "YES" %}
               BẬT
            {% else %}
               TẮT
            {% endif %}
        </span></p>
        <button onclick="toggleLivingLED()">
            {% if sensor_data.living_room.led == "ON" %}
                TẮT ĐÈN
            {% else %}
                BẬT ĐÈN
            {% endif %}
        </button>
        <button onclick="toggleLivingFan()">
            {% if sensor_data.living_room.fan == "ON" %}
                TẮT QUẠT
            {% else %}
                BẬT QUẠT
            {% endif %}
        </button>
        <button onclick="toggleLivingSensor()">
            {% if sensor_data.living_room.sensor == "YES" %}
                TẮT CẢM BIẾN
            {% else %}
                BẬT CẢM BIẾN
            {% endif %}
        </button>
    </div>

    <!-- PHÒNG NGỦ -->
    <div class="container">
        <h1>Phòng Ngủ</h1>
        <p>Nhiệt độ: {{ sensor_data.bedroom.temp }} °C</p>
        <p>Độ ẩm: {{ sensor_data.bedroom.hum }} %</p>
        <p>Mưa: {{ sensor_data.bedroom.rain }}</p>
        <p>Đèn LED: <span id="bedroom_led">{{ sensor_data.bedroom.led }}</span></p>
        <p>Quạt: <span id="bedroom_fan">{{ sensor_data.bedroom.fan }}</span></p>
        <p>Cửa sổ: <span id="bedroom_window">{{ sensor_data.bedroom.window }}</span></p>
        <button onclick="toggleBedroomLED()">
            {% if sensor_data.bedroom.led == "ON" %}
                TẮT ĐÈN
            {% else %}
                BẬT ĐÈN
            {% endif %}
        </button>
        <button onclick="toggleBedroomFan()">
            {% if sensor_data.bedroom.fan == "ON" %}
                TẮT QUẠT
            {% else %}
                BẬT QUẠT
            {% endif %}
        </button>
        <button onclick="toggleBedroomWindow()">
            {% if sensor_data.bedroom.window == "OPEN" %}
                ĐÓNG CỬA SỔ
            {% else %}
                MỞ CỬA SỔ
            {% endif %}
        </button>
    </div>
</body>
</html>
```

## 🚀 Hướng dẫn sử dụng

- **Sau khi nạp chương trình cho các module:**  
  Các board sẽ tự động kết nối WiFi và gửi dữ liệu cảm biến về Server Flask, đồng thời đồng bộ trạng thái thiết bị.

- **Trên giao diện Web:**  
  Người dùng có thể theo dõi dữ liệu cập nhật (nhiệt độ, độ ẩm, khí gas, chuyển động, mưa, …) và điều khiển thiết bị (bật/tắt LED, quạt, mở/đóng cửa sổ) cho từng phòng thông qua các nút điều khiển.

- **Quản lý truy cập của Cửa Ra Vào:**  
  Hệ thống sẽ cho phép mở cửa khi thẻ RFID hợp lệ được quét hoặc khi người dùng nhập mật khẩu đúng từ bàn phím.

## 📚 Tài liệu

- [Hướng dẫn cài đặt và cấu hình chi tiết](docs/installation.md)
- [Hướng dẫn sử dụng giao diện web & API](docs/user-manual.md)
- [Tài liệu lập trình module cho từng phòng](docs/module_reference.md)

## 📝 License

© 2024 AIoTLab – Faculty of Information Technology, DaiNam University.  
Tất cả các quyền được bảo lưu.

---

<div align="center">
  <p>Được xây dựng với 💡 bởi AIoTLab tại Đại Học Đà Nẵng</p>
  <p>
    <a href="https://fit.dainam.edu.vn">Website</a> • 
    <a href="https://github.com/drkhanusa">GitHub</a> • 
    <a href="mailto:contact@dainam.edu.vn">Liên hệ</a>
  </p>
</div>
```
