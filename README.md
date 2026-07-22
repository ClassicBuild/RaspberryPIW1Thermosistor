### Install W1ThermSensor with Async Support

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Installs the w1thermsensor package with the 'async' extra, which adds support for asyncio and the AsyncW1ThermSensor class.

```bash
pip install w1thermsensor[async]
```

--------------------------------

### Install W1ThermSensor

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Installation commands for the package, including optional async support and system-level packages for Raspbian.

```bash
# Standard installation
pip install w1thermsensor

# With async support for AsyncW1ThermSensor
pip install w1thermsensor[async]

# On Raspbian
sudo apt-get install python3-w1thermsensor
```

--------------------------------

### Build and Install Debian Package

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Manually builds a Debian package for the w1thermsensor module and installs it. This method is useful for custom builds or when installing from source.

```bash
debuild -us -uc
dpkg -i ../python3-w1thermsensor_*.deb
```

--------------------------------

### Install W1ThermSensor on Raspbian

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Installs the w1thermsensor module on Raspberry Pi running Raspbian using the apt-get package manager. This is a convenient method for Raspbian users.

```bash
sudo apt-get install python3-w1thermsensor
```

--------------------------------

### Basic Temperature Reading (Celsius and Fahrenheit)

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Demonstrates basic usage of the W1ThermSensor class to get temperature readings in Celsius and Fahrenheit. The first detected sensor is used by default. Exceptions are raised if an error occurs during initialization or reading.

```python
from w1thermsensor import W1ThermSensor, Unit

sensor = W1ThermSensor()
temperature_in_celsius = sensor.get_temperature()
temperature_in_fahrenheit = sensor.get_temperature(Unit.DEGREES_F)
```

--------------------------------

### Get Temperature in Multiple Units

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Shows how to retrieve temperature readings simultaneously in Celsius, Fahrenheit, and Kelvin using the get_temperatures method. This allows for easy comparison or use in different contexts.

```python
from w1thermsensor import W1ThermSensor, Unit

sensor = W1ThermSensor()
temperature_in_all_units = sensor.get_temperatures([
    Unit.DEGREES_C,
    Unit.DEGREES_F,
    Unit.KELVIN])
```

--------------------------------

### AsyncW1ThermSensor Class for Asynchronous Operations

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Provides an asyncio-compatible interface for reading temperature sensors asynchronously. Requires the `async` extra: `pip install w1thermsensor[async]`.

```python
import asyncio
from w1thermsensor import AsyncW1ThermSensor, Unit

async def read_temperature():
    sensor = AsyncW1ThermSensor()

    # Get temperature asynchronously
    temp = await sensor.get_temperature()
    print(f"Temperature: {temp:.2f}C")

    # Get temperature in Fahrenheit
    temp_f = await sensor.get_temperature(Unit.DEGREES_F)
    print(f"Temperature: {temp_f:.2f}F")

    # Get temperatures in multiple units
    temps = await sensor.get_temperatures([Unit.DEGREES_C, Unit.DEGREES_F, Unit.KELVIN])
    print(f"Temps: {temps}")

    # Get resolution
    resolution = await sensor.get_resolution()
    print(f"Resolution: {resolution} bits")

asyncio.run(read_temperature())
```

--------------------------------

### Verify Hardware Connection

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Lists devices in the w1 bus, useful for verifying hardware connections. Filenames starting with '28-' indicate detected sensors, while '00-' may suggest a missing pull-up resistor.

```bash
ls -l /sys/bus/w1/devices
```

--------------------------------

### Async W1ThermSensor Interface

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Utilize the asynchronous interface for non-blocking temperature readings with asyncio. Supports getting single or multiple temperatures in various units.

```python
from w1thermsensor import AsyncW1ThermSensor, Unit

sensor = AsyncW1ThermSensor()
temperature_in_celsius = await sensor.get_temperature()
temperature_in_fahrenheit = await sensor.get_temperature(Unit.DEGREES_F)
temperature_in_all_units = await sensor.get_temperatures([
    Unit.DEGREES_C,
    Unit.DEGREES_F,
    Unit.KELVIN])
```

--------------------------------

### Set and Get Temperature Offset

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Applies a temperature offset to calibrate sensor readings. The offset is added to each temperature reading. Supports Celsius and Fahrenheit units.

```python
from w1thermsensor import W1ThermSensor, Unit

sensor = W1ThermSensor()

# Set offset in Celsius (add 0.5C to all readings)
sensor.set_offset(0.5, Unit.DEGREES_C)

# Set offset in Fahrenheit
sensor.set_offset(1.0, Unit.DEGREES_F)

# Get current offset
offset = sensor.get_offset(Unit.DEGREES_C)
print(f"Current offset: {offset}C")

# Temperature readings now include the offset
temp = sensor.get_temperature()
print(f"Calibrated temperature: {temp:.2f}C")
```

--------------------------------

### Initialize W1ThermSensor

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Demonstrates various ways to instantiate the W1ThermSensor class, including auto-detection, specific IDs, types, and calibration offsets.

```python
from w1thermsensor import W1ThermSensor, Sensor, Unit

# Initialize with first available sensor (auto-detect)
sensor = W1ThermSensor()

# Initialize with specific sensor type
sensor = W1ThermSensor(sensor_type=Sensor.DS18B20)

# Initialize with specific sensor ID
sensor = W1ThermSensor(sensor_id="00000588806a")

# Initialize with both type and ID
sensor = W1ThermSensor(sensor_type=Sensor.DS18B20, sensor_id="00000588806a")

# Initialize with temperature offset for calibration
sensor = W1ThermSensor(offset=0.5, offset_unit=Unit.DEGREES_C)

# Access sensor properties
print(f"Sensor ID: {sensor.id}")          # e.g., "00000588806a"
print(f"Sensor Type: {sensor.name}")       # e.g., "DS18B20"
print(f"Exists: {sensor.exists()}")        # True/False
```

--------------------------------

### CLI: List Sensors

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

List all available W1ThermSensors using the command-line tool. Supports filtering by sensor type and outputting in JSON format.

```bash
$ w1thermsensor ls
```

```bash
$ w1thermsensor ls --json  # show results in JSON format
```

```bash
$ w1thermsensor ls --type DS1822
```

```bash
$ w1thermsensor ls --type DS1822 --type MAX31850K  # specify multiple sensor types
```

```bash
$ w1thermsensor ls --type DS1822 --json  # show results in JSON format
```

--------------------------------

### Async Producer-Consumer Pattern for Multiple Sensors

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Demonstrates reading multiple sensors concurrently using asyncio with a producer-consumer pattern. This pattern efficiently handles readings from various sensors.

```python
import asyncio
import time
from dataclasses import dataclass
from w1thermsensor import AsyncW1ThermSensor

@dataclass
class Reading:
    sensor_id: str
    timestamp: float
    temperature: float

async def produce_readings(sensor: AsyncW1ThermSensor, queue: asyncio.Queue):
    """Async producer that reads temperature from a single sensor"""
    while True:
        timestamp = time.time()
        temperature = await sensor.get_temperature()
        await queue.put(Reading(sensor.id, timestamp, temperature))
        await asyncio.sleep(1)

async def consume_readings(queue: asyncio.Queue):
    """Async consumer that processes temperature readings"""
    while True:
        reading = await queue.get()
        print(f"[{reading.timestamp:.2f}] Sensor {reading.sensor_id}: {reading.temperature:.2f}C")
        queue.task_done()

async def main():
    queue = asyncio.Queue()

    # Create a producer for each available sensor
    sensors = AsyncW1ThermSensor.get_available_sensors()
    producers = [produce_readings(sensor, queue) for sensor in sensors]

    # Create consumer
    consumer = consume_readings(queue)

    # Run all tasks concurrently
    await asyncio.gather(*producers, consumer)

asyncio.run(main())
```

--------------------------------

### CLI: List Available Sensors

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Use the command-line interface to list connected sensors with optional resolution, type filtering, or JSON output.

```bash
# List all available sensors
$ w1thermsensor ls
Found 2 sensors:
  1. HWID: 00000588806a Type: DS18B20
  2. HWID: 00000588123b Type: DS18B20

# List sensors with resolution info
$ w1thermsensor ls --resolution
Found 2 sensors:
  1. HWID: 00000588806a Type: DS18B20 Resolution: 12
  2. HWID: 00000588123b Type: DS18B20 Resolution: 12

# List only specific sensor types
$ w1thermsensor ls --type DS18B20

# List multiple specific types
$ w1thermsensor ls --type DS18B20 --type DS1822

# Output as JSON
$ w1thermsensor ls --json
[
    {
        "hwid": "00000588806a",
        "id": 1,
        "type": "DS18B20"
    },
    {
        "hwid": "00000588123b",
        "id": 2,
        "type": "DS18B20"
    }
]
```

--------------------------------

### W1ThermSensor Class Initialization

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Methods for initializing the W1ThermSensor class to interact with connected hardware sensors.

```APIDOC
## W1ThermSensor Initialization

### Description
Initializes the sensor interface. Can auto-detect the first available sensor or target a specific sensor by type or ID.

### Parameters
#### Request Body
- **sensor_type** (Sensor) - Optional - The specific model of the sensor (e.g., Sensor.DS18B20).
- **sensor_id** (string) - Optional - The unique hardware ID of the sensor.
- **offset** (float) - Optional - Temperature offset for calibration.
- **offset_unit** (Unit) - Optional - The unit for the provided offset.
```

--------------------------------

### CLI: Show Temperatures

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Display temperatures from W1ThermSensors via the CLI. Can show all sensors or a specific one by ID or hardware ID. Supports filtering by type and JSON output.

```bash
$ w1thermsensor all --type DS1822
```

```bash
$ w1thermsensor all --type DS1822 --type MAX31850K  # specify multiple sensor types
```

```bash
$ w1thermsensor all --type DS1822 --json  # show results in JSON format
```

```bash
$ w1thermsensor get 1  # 1 is the id obtained by the ls command
```

```bash
$ w1thermsensor get --hwid 00000588806a --type DS18B20
```

```bash
$ w1thermsensor get 1  # show results in JSON format
```

--------------------------------

### Calibrate W1ThermSensor

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Instantiate a W1ThermSensor with calibration data to obtain corrected temperature readings. Ensure measured and reference high/low points are accurately determined.

```python
from w1thermsensor.calibration_data import CalibrationData
from w1thermsensor import W1ThermSensor, Unit

calibration_data = CalibrationData(
        measured_high_point=measured_high_point,
        measured_low_point=measured_low_point,
        reference_high_point=reference_high_point,
        reference_low_point=reference_low_point, # optional, defaults to 0.0
    )
sensor = W1ThermSensor(calibration_data=calibration_data)

corrected_temperature_in_celsius = sensor.get_corrected_temperature()
corrected_temperature_in_fahrenheit = sensor.get_corrected_temperature(Unit.DEGREES_F)
corrected_temperature_in_all_units = sensor.get_corrected_temperatures([
    Unit.DEGREES_C,
    Unit.DEGREES_F,
    Unit.KELVIN])
```

--------------------------------

### Two-Point Calibration with CalibrationData

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Performs two-point calibration using measured ice point and boiling point values against reference values to correct sensor inaccuracies across the entire temperature range. Initialize the sensor with CalibrationData.

```python
from w1thermsensor import W1ThermSensor, Unit
from w1thermsensor.calibration_data import CalibrationData

# Create calibration data from measured values
# measured_low_point: sensor reading in ice water (~0C)
# measured_high_point: sensor reading in boiling water (~100C at sea level)
# reference_high_point: actual boiling point at your altitude
# reference_low_point: actual freezing point (default 0.0)
calibration_data = CalibrationData(
    measured_high_point=99.2,    # Sensor read 99.2C in boiling water
    measured_low_point=0.3,      # Sensor read 0.3C in ice water
    reference_high_point=100.0,  # Actual boiling point at sea level
    reference_low_point=0.0      # Actual freezing point
)

# Initialize sensor with calibration data
sensor = W1ThermSensor(calibration_data=calibration_data)

# Get corrected temperature
corrected_temp = sensor.get_corrected_temperature()
print(f"Corrected temperature: {corrected_temp:.2f}C")

# Get corrected temperature in other units
corrected_fahrenheit = sensor.get_corrected_temperature(Unit.DEGREES_F)
print(f"Corrected temperature: {corrected_fahrenheit:.2f}F")

# Get corrected temperature in multiple units
corrected_temps = sensor.get_corrected_temperatures([
    Unit.DEGREES_C,
    Unit.DEGREES_F,
    Unit.KELVIN
])
print(f"Celsius: {corrected_temps[0]:.2f}C")
print(f"Fahrenheit: {corrected_temps[1]:.2f}F")
print(f"Kelvin: {corrected_temps[2]:.2f}K")
```

--------------------------------

### Sensor Configuration and Calibration

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Methods for setting sensor resolution, applying temperature offsets, and performing two-point calibration.

```APIDOC
## set_resolution(bits, persist)

### Description
Sets the resolution of the sensor. Note that EEPROM has limited writes (~50k) and requires root privileges if persist is True.

### Parameters
- **bits** (int) - Required - Resolution in bits.
- **persist** (bool) - Optional - If True, saves to EEPROM to survive power cycles.

## set_offset(offset, unit)

### Description
Applies a temperature offset to calibrate sensor readings. The offset is added to each reading.

### Parameters
- **offset** (float) - Required - The value to add to the reading.
- **unit** (Unit) - Required - The unit of the offset (e.g., Unit.DEGREES_C).

## CalibrationData(measured_high_point, measured_low_point, reference_high_point, reference_low_point)

### Description
Provides two-point calibration using measured ice/boiling points compared to reference values.

### Parameters
- **measured_high_point** (float) - Required - Sensor reading in boiling water.
- **measured_low_point** (float) - Required - Sensor reading in ice water.
- **reference_high_point** (float) - Required - Actual boiling point at altitude.
- **reference_low_point** (float) - Required - Actual freezing point.
```

--------------------------------

### set_resolution() and get_resolution()

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Configures or retrieves the precision of the temperature sensor.

```APIDOC
## set_resolution() / get_resolution()

### Description
Configure the sensor's temperature reading resolution (9-12 bits). Higher resolution provides more precision but takes longer to read.

### Parameters
#### Request Body
- **resolution** (int) - Required (for set) - The resolution in bits (9, 10, 11, or 12).
```

--------------------------------

### Exception Handling

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Handle specific error conditions like missing sensors, unready states, or invalid configurations.

```python
from w1thermsensor import W1ThermSensor, Sensor
from w1thermsensor.errors import (
    W1ThermSensorError,
    NoSensorFoundError,
    SensorNotReadyError,
    ResetValueError,
    KernelModuleLoadError,
    UnsupportedUnitError,
    InvalidCalibrationDataError
)

try:
    sensor = W1ThermSensor(sensor_type=Sensor.DS18B20, sensor_id="nonexistent")
    temp = sensor.get_temperature()
except NoSensorFoundError as e:
    print(f"Sensor not found: {e}")
    # Check cabling and /boot/config.txt for dtoverlay=w1-gpio

try:
    sensor = W1ThermSensor()
    temp = sensor.get_temperature()
except SensorNotReadyError as e:
    print(f"Sensor not ready: {e}")
    # Retry after a short delay

try:
    sensor = W1ThermSensor()
    temp = sensor.get_temperature()
except ResetValueError as e:
    print(f"Reset value detected: {e}")
    # Check power supply to sensor (85C indicates power issue)

try:
    sensor = W1ThermSensor()
    sensor.set_resolution(15)  # Invalid resolution
except ValueError as e:
    print(f"Invalid resolution: {e}")  # Must be 9-12
```

--------------------------------

### Set Sensor Resolution via CLI

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Commands to configure the resolution of a sensor using either its index or hardware ID and type.

```bash
$ sudo w1thermsensor resolution 10 1
```

```bash
$ sudo w1thermsensor resolution 11 --hwid 00000588806a --type DS18B20
```

--------------------------------

### Configure Soft Pull-up with RPi.GPIO

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

This code configures a soft pull-up resistor for GPIO pin 4 using the RPi.GPIO library. This is an alternative to hardware pull-ups or kernel configurations. Note that 1-Wire devices are only visible to the kernel while this program actively pulls the GPIO pin up.

```python
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BCM)
GPIO.setup(4, GPIO.IN, pull_up_down=GPIO.PUD_UP)
```

--------------------------------

### CLI: Read All Sensor Temperatures

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Read temperatures from all connected sensors with support for unit conversion, filtering, and JSON output.

```bash
# Read all sensors in Celsius (default)
$ w1thermsensor all
Got temperatures of 2 sensors:
  Sensor 1 (00000588806a) measured temperature: 23.45 celsius
  Sensor 2 (00000588123b) measured temperature: 24.12 celsius

# Read in Fahrenheit
$ w1thermsensor all --unit fahrenheit
Got temperatures of 2 sensors:
  Sensor 1 (00000588806a) measured temperature: 74.21 fahrenheit
  Sensor 2 (00000588123b) measured temperature: 75.42 fahrenheit

# Read only specific sensor types
$ w1thermsensor all --type DS18B20

# Read with specific resolution
$ w1thermsensor all --resolution 10

# Output as JSON
$ w1thermsensor all --json
[
    {
        "hwid": "00000588806a",
        "id": 1,
        "temperature": 23.45,
        "type": "DS18B20",
        "unit": "celsius"
    }
]
```

--------------------------------

### Read temperature from a specific sensor

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Initialize a specific sensor by type and ID to retrieve its current temperature.

```python
from w1thermsensor import W1ThermSensor, Sensor

sensor = W1ThermSensor(sensor_type=Sensor.DS18B20, sensor_id="00000588806a")
temperature_in_celsius = sensor.get_temperature()
```

--------------------------------

### Accessing Sensor and Unit Enums

Source: https://github.com/timofurrer/w1thermsensor/blob/master/CHANGELOG.rst

Use the Sensor and Unit enums to identify supported hardware and temperature units.

```python
from w1thermsensor import Sensor

print(Sensor.DS18B20)
```

```python
from w1thermsensor import Unit

print(Unit.DEGREES_F)
```

--------------------------------

### Configure sensor resolution

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Adjust the sensor resolution, optionally persisting the setting to EEPROM.

```python
sensor = W1ThermSensor(sensor_type=Sensor.DS18B20, sensor_id="00000588806a")
sensor.set_resolution(9)
```

```python
sensor = W1ThermSensor(sensor_type=Sensor.DS18B20, sensor_id="00000588806a")
sensor.set_resolution(9, persist=True)
```

--------------------------------

### get_available_sensors()

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Lists all connected 1-Wire sensors detected by the system.

```APIDOC
## get_available_sensors()

### Description
Class method that returns a list of all available w1 therm sensors connected to the system. Can filter by sensor type.

### Parameters
#### Query Parameters
- **sensor_types** (list) - Optional - A list of Sensor types to filter the results by.
```

--------------------------------

### Configure Sensor Resolution

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Manages the sensor's resolution settings, which affects precision and conversion time.

```python
from w1thermsensor import W1ThermSensor, Sensor

sensor = W1ThermSensor(sensor_type=Sensor.DS18B20, sensor_id="00000588806a")

# Get current resolution
current_resolution = sensor.get_resolution()
print(f"Current resolution: {current_resolution} bits")  # Output: Current resolution: 12 bits

# Set resolution to 9 bits (fastest, ~93.75ms, 0.5C precision)
sensor.set_resolution(9)

# Set resolution to 10 bits (~187.5ms, 0.25C precision)
sensor.set_resolution(10)

# Set resolution to 11 bits (~375ms, 0.125C precision)
sensor.set_resolution(11)

# Set resolution to 12 bits (slowest, ~750ms, 0.0625C precision)
sensor.set_resolution(12)
```

--------------------------------

### Test Temperature Reading

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Iterates through detected w1 therm sensors and prints their raw temperature data from the w1_slave file. This command helps test if sensors are correctly communicating temperature readings.

```bash
for i in /sys/bus/w1/devices/28-*; do cat $i/w1_slave; done
```

--------------------------------

### CLI: Set Sensor Resolution

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Change the temperature read resolution of a W1ThermSensor and write the setting to its EEPROM. Requires sensor ID and desired resolution.

```bash
# w1thermsensor resolution 10 1
```

```bash
$ w1thermsensor get 1 --resolution 10
```

```bash
$ w1thermsensor get --hwid 00000588806a --type DS18B20 --resolution 11
```

--------------------------------

### Read Temperature

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Retrieves temperature readings in Celsius, Fahrenheit, or Kelvin using the get_temperature method.

```python
from w1thermsensor import W1ThermSensor, Unit

sensor = W1ThermSensor()

# Get temperature in Celsius (default)
temp_celsius = sensor.get_temperature()
print(f"Temperature: {temp_celsius:.2f}C")  # Output: Temperature: 23.45C

# Get temperature in Fahrenheit
temp_fahrenheit = sensor.get_temperature(Unit.DEGREES_F)
print(f"Temperature: {temp_fahrenheit:.2f}F")  # Output: Temperature: 74.21F

# Get temperature in Kelvin
temp_kelvin = sensor.get_temperature(Unit.KELVIN)
print(f"Temperature: {temp_kelvin:.2f}K")  # Output: Temperature: 296.60K
```

--------------------------------

### List Available Sensors

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Retrieves a list of all connected sensors, with optional filtering by sensor type.

```python
from w1thermsensor import W1ThermSensor, Sensor

# Get all available sensors
all_sensors = W1ThermSensor.get_available_sensors()
print(f"Found {len(all_sensors)} sensors")

for sensor in all_sensors:
    temp = sensor.get_temperature()
    print(f"Sensor {sensor.id} ({sensor.name}): {temp:.2f}C")

# Get only DS18B20 sensors
ds18b20_sensors = W1ThermSensor.get_available_sensors([Sensor.DS18B20])
for sensor in ds18b20_sensors:
    print(f"DS18B20 {sensor.id}: {sensor.get_temperature():.2f}C")

# Get multiple specific sensor types
specific_sensors = W1ThermSensor.get_available_sensors([Sensor.DS18B20, Sensor.DS1822])
```

--------------------------------

### Persist Resolution to EEPROM

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Saves the sensor's resolution to EEPROM, which survives power cycles. Note that EEPROM has limited write cycles (~50k) and requires root privileges.

```python
sensor.set_resolution(10, persist=True)
```

--------------------------------

### Iterate over available sensors

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

List and read from all connected sensors or filter by specific sensor types.

```python
from w1thermsensor import W1ThermSensor

for sensor in W1ThermSensor.get_available_sensors():
    print("Sensor %s has temperature %.2f" % (sensor.id, sensor.get_temperature()))
```

```python
from w1thermsensor import W1ThermSensor, Sensor

for sensor in W1ThermSensor.get_available_sensors([Sensor.DS18B20]):
    print("Sensor %s has temperature %.2f" % (sensor.id, sensor.get_temperature()))
```

--------------------------------

### CLI: Read Single Sensor Temperature

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Read temperature from a specific sensor using index, hardware ID, or type, with support for offsets and units.

```bash
# Read by sensor index (from ls command)
$ w1thermsensor get 1
Sensor 00000588806a measured temperature: 23.45 celsius

# Read by hardware ID and type
$ w1thermsensor get --hwid 00000588806a --type DS18B20
Sensor 00000588806a measured temperature: 23.45 celsius

# Read in different units
$ w1thermsensor get 1 --unit fahrenheit
$ w1thermsensor get 1 --unit kelvin

# Read with specific resolution
$ w1thermsensor get 1 --resolution 9

# Read with temperature offset
$ w1thermsensor get 1 --offset 0.5

# Output as JSON
$ w1thermsensor get 1 --json
{
    "hwid": "00000588806a",
    "offset": 0.0,
    "temperature": 23.45,
    "type": "DS18B20",
    "unit": "celsius"
}
```

--------------------------------

### Read Multiple Units

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Fetches temperature readings in multiple units simultaneously from a single sensor read operation.

```python
from w1thermsensor import W1ThermSensor, Unit

sensor = W1ThermSensor()

# Get temperature in all units with a single read
temps = sensor.get_temperatures([Unit.DEGREES_C, Unit.DEGREES_F, Unit.KELVIN])

celsius, fahrenheit, kelvin = temps
print(f"Celsius: {celsius:.2f}C")       # Output: Celsius: 23.45C
print(f"Fahrenheit: {fahrenheit:.2f}F") # Output: Fahrenheit: 74.21F
print(f"Kelvin: {kelvin:.2f}K")         # Output: Kelvin: 296.60K
```

--------------------------------

### AsyncW1ThermSensor API

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Asynchronous interface for reading temperature sensors using asyncio.

```APIDOC
## get_temperature(unit)

### Description
Asynchronously retrieves the current temperature from the sensor.

### Parameters
- **unit** (Unit) - Optional - The unit to return the temperature in.

## get_temperatures(units)

### Description
Asynchronously retrieves temperatures in multiple units.

### Parameters
- **units** (list) - Required - A list of Unit types to return.

## get_resolution()

### Description
Asynchronously retrieves the sensor resolution in bits.
```

--------------------------------

### Sensor Types Enumeration

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Imports the Sensor enum which defines all supported w1 therm sensor types.

```python
from w1thermsensor import Sensor
```

--------------------------------

### get_temperature()

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Retrieves the current temperature reading from a sensor instance.

```APIDOC
## get_temperature()

### Description
Returns the current temperature reading from the sensor in the specified unit.

### Parameters
#### Query Parameters
- **unit** (Unit) - Optional - The unit for the temperature (Celsius, Fahrenheit, Kelvin). Defaults to Celsius.
```

--------------------------------

### Disable kernel module auto-loading

Source: https://github.com/timofurrer/w1thermsensor/blob/master/README.md

Prevent automatic loading of kernel modules by setting the W1THERMSENSOR_NO_KERNEL_MODULE environment variable.

```bash
# set it globally for your shell so that sub-processes will inherit it.
export W1THERMSENSOR_NO_KERNEL_MODULE=1

# set it just for your Python process
W1THERMSENSOR_NO_KERNEL_MODULE=1 python my_awesome_thermsensor_script.py
```

--------------------------------

### Disable Kernel Module Auto-Loading

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Methods to prevent automatic loading of w1 kernel modules for shell environments and Python scripts.

```bash
export W1THERMSENSOR_NO_KERNEL_MODULE=1
```

```bash
W1THERMSENSOR_NO_KERNEL_MODULE=1 python my_script.py
```

```bash
W1THERMSENSOR_NO_KERNEL_MODULE=1 w1thermsensor ls
```

```python
import os
os.environ["W1THERMSENSOR_NO_KERNEL_MODULE"] = "1"

# Now import w1thermsensor - kernel modules won't be auto-loaded
from w1thermsensor import W1ThermSensor

# You must ensure w1-gpio and w1-therm modules are loaded manually:
# sudo modprobe w1-gpio
# sudo modprobe w1-therm
```

--------------------------------

### Temperature Unit Conversion

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Use the Unit enum to handle temperature conversions between Celsius, Fahrenheit, and Kelvin.

```python
from w1thermsensor import Unit

# Available units
print(Unit.DEGREES_C)   # celsius
print(Unit.DEGREES_F)   # fahrenheit
print(Unit.KELVIN)      # kelvin

# Get conversion function between units
convert_c_to_f = Unit.get_conversion_function(Unit.DEGREES_C, Unit.DEGREES_F)
fahrenheit = convert_c_to_f(25.0)
print(f"25C = {fahrenheit}F")  # Output: 25C = 77.0F

convert_c_to_k = Unit.get_conversion_function(Unit.DEGREES_C, Unit.KELVIN)
kelvin = convert_c_to_k(25.0)
print(f"25C = {kelvin}K")  # Output: 25C = 298.15K
```

--------------------------------

### Sensor Type Identification

Source: https://context7.com/timofurrer/w1thermsensor/llms.txt

Access sensor type constants and validate 12-bit standard compliance or parse types from ID strings.

```python
print(Sensor.DS18S20)    # 0x10
print(Sensor.DS1822)     # 0x22
print(Sensor.DS18B20)    # 0x28
print(Sensor.DS28EA00)   # 0x42
print(Sensor.DS1825)     # 0x3B
print(Sensor.MAX31850K)  # 0x3B

# Check if sensor complies with 12-bit standard
sensor_type = Sensor.DS18B20
if sensor_type.comply_12bit_standard():
    print(f"{sensor_type.name} supports 12-bit resolution")

# Parse sensor type from ID string
sensor_type = Sensor.from_id_string("28")  # Returns Sensor.DS18B20
```
