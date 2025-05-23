## Installation

### Prerequisites
The package requires QtSerialPort development package from Qt 5.2+
```sh
sudo apt-get install libqt5serialport5-dev
```

### Building
```sh
cd ros2_ws/src
git clone --recursive https://github.com/cornellev/witomotion_imu_ros2.git witmotion_ros
colcon build --packages-select witmotion_ros
source install/setup.bash
```
If compilation fails, first check the directory `src/witmotion_ros/witmotion-uart-qt`. If it is empty, the recursive clone failed, and you should manually clone the underlying library from the repository [https://github.com/ElettraSciComp/witmotion_IMU_QT](https://github.com/cornellev/witmotion_IMU_QT) into this directory.

### Parameters
- `port` - the virtual kernel device name for a port, `ttyUSB0` by default
- `baud_rate` - port rate value to be used by the library for opening the port, _9600 baud_ by default
- `polling_interval` - the sensor polling interval in milliseconds. If this parameter is omitted, the default value is set up by the library (50 ms).
- `timeout_ms` - the sensor timeout period in milliseconds.  If no data is received from the sensor after this period, then an error is raised and the node terminates. If this parameter is omitted, a default value of 3 times the polling interval is used.  If this parameter is zero, the timeout check is disabled.
- `restart_service_name` - the service name used to restart the sensor connection after an error.
- `imu_publisher:`
    - `topic_name` - the topic name for IMU data publisher, `imu` in the node's namespace by default
    - `frame_id` - IMU message header [frame ID](https://wiki.ros.org/tf)
    - `use_native_orientation` - instructs the node to use the native quaternion orientation measurement from the sensor instead of synthesized from Euler angles. **NOTE**: if this setting is enabled bu the sensor does not produce orientation in the quaternion format, the IMU message will never be published!
    - `measurements` - every measurement in IMU message data pack can be enabled or disabled. If the measurement is disabled, the corresponding covariance matrix is set to begin from `-1` as it is described in the [message definition](https://docs.ros.org/en/lunar/api/sensor_msgs/html/msg/Imu.html).
        - `acceleration`
            - `enabled`
            - `covariance` - row-major matrix 3x3, all zeros for unknown covariation
        - `angular_velocity`
            - `enabled`
            - `covariance` - row-major matrix 3x3, all zeros for unknown covariation
        - `orientation`
            - `enabled`
            - `covariance` - row-major matrix 3x3, all zeros for unknown covariation
- `temperature_publisher`
    - `enabled` - enable or disable temperature measurement extraction
    - `topic_name` - the topic name for publishing temperature data
    - `frame_id` - message header frame ID
    - `from_message` - the message type string to determine from which type of Witmotion measurement message the temperature data should be extracted (please refer to the original documentation for detailed description). The possible values are: `acceleration`, `angular_vel`, `orientation` or `magnetometer`.
    - `variance` - the constant variance, if applicable, otherwise 0
    - `coefficient` - linear calibration multiplier, 1.0 by default
    - `addition` - linear calibration addendum, 0 by default
- `magnetometer_publisher`
    - `enabled` - enable or disable magnetometer measurement extraction
    - `topic_name` - the topic name for publishing the data
    - `frame_id` - message header frame ID
    - `coefficient` - linear calibration multiplier, 1.0 by default
    - `addition` - linear calibration addendum, 0 by default
    - `covariance` - row-major matrix 3x3, all zeros for unknown covariation
- `barometer_publisher`
    - `enabled` - enable or disable barometer measurement extraction
    - `topic_name` - the topic name for publishing the data
    - `frame_id` - message header frame ID
    - `coefficient` - linear calibration multiplier, 1.0 by default
    - `addition` - linear calibration addendum, 0 by default
    - `variance` - the constant variance, if applicable, otherwise 0
- `altimeter_publisher`
    - `enabled` - enable or disable altitude measurement extraction
    - `topic_name` - the topic name for publishing the data
    - `coefficient` - linear calibration multiplier, 1.0 by default
    - `addition` - linear calibration addendum, 0 by default
- `orientation_publisher`
    - `enabled` - enable or disable orientation measurement extraction
    - `topic_name` - the topic name for publishing the data
- `gps_publisher`
    - `enabled` - enables/disables all GPS receiver measurements extraction
    - `navsat_fix_frame_id` - frame ID for GPS fixed position publisher
    - `navsat_fix_topic_name` - topic name for GPS fixed position publisher
    - `navsat_altitude_topic_name` - topic name for GPS altitude publisher
    - `navsat_satellites_topic_name` - topic name for GPS active satellites number publisher
    - `navsat_variance_topic_name` - topic name for GPS diagonal variance publisher
    - `ground_speed_topic_name` - topic name for GPS ground speed publisher
- `rtc_publisher`
    - `enabled` - enables/disables realtime clock information decoder
    - `topic_name` - topic name for realtime clock publisher
    - `presync` - instructs the node to perform an attempt to pre-synchronize sensor's internal realtime clock

