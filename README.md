# Flask IoT Data Processing API

This is a Flask-based API designed for receiving and processing IoT data from gateway and module devices. The API processes data fields like temperature, humidity, pressure, and more, and also retrieves location information based on the gateway's IP address.

## Features

- Receives data from gateway and module devices in JSON format.
- Calculates additional metrics such as battery level, battery life, acceleration, angular velocity, and stability.
- Retrieves IP-based geolocation information using the [ipinfo.io](https://ipinfo.io/) API.
- Stores the latest data in memory and displays it at the root endpoint.

## Requirements

- Python 3.x
- Flask
- ipinfo
- math
- (For Windows users) pywin32, installed automatically if on Windows.

## Installation

1. **Clone the repository**:
    ```bash
    git clone https://github.com/<your-username>/SimpleJavaProject.git
    cd SimpleJavaProject
    ```

2. **Install dependencies**:
    ```bash
    pip install flask ipinfo
    ```

3. **Setup IPInfo Access Token**:
    - Update the `access_token` variable in `get_ip_info` function with your own IPInfo access token. You can get one by signing up at [ipinfo.io](https://ipinfo.io/).

## Usage

1. **Run the application**:
    ```bash
    python app.py
    ```

2. **Endpoints**:
   - **GET /**: Returns the latest received data.
   - **POST /data**: Receives gateway and module data in JSON format, processes it, and returns the processed data.

3. **Data Processing**:
   - The application calculates metrics like:
     - Battery level and battery life.
     - Acceleration (`acc`) and angular velocity (`ang_veloc`) using accelerometer and gyroscope data.
     - Stability of the module based on thresholds for acceleration and angular velocity.
     - A placeholder `bme_prediction` function for future expansion.

4. **IP Geolocation**:
   - The gateway IP address is used to fetch location details such as city, region, country, and ISP. If the IP address is not provided, the IP information fields remain empty.

## Example Usage with `curl`

- **Send Data**:
    ```bash
    curl -X POST http://localhost:5000/data -H "Content-Type: application/json" -d '{
        "gateway_data": {
            "Module_id": "gateway123",
            "Ip_address": "8.8.8.8",
            "Temperature": 25.5,
            "Humidity": 60,
            "Pressure": 1013,
            "Voltage": 3.7
        },
        "module_data": [
            {
                "Module_id": "module456",
                "Temperature": 22.4,
                "Humidity": 55,
                "Pressure": 1012,
                "Gas_resistance": 120,
                "Gas_index": 2,
                "Meas_index": 5,
                "Ax": 0.02,
                "Ay": 0.03,
                "Az": 1.0,
                "Gx": 0.01,
                "Gy": 0.01,
                "Gz": 0.01
            }
        ]
    }'
    ```

- **Get Latest Data**:
    ```bash
    curl http://localhost:5000/
    ```

## Project Structure

```plaintext
.
├── app.py            # Main Flask application file
├── README.md         # Documentation
└── requirements.txt  # Python dependencies (if created)
