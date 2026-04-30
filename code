from machine import Pin, ADC
import dht
import time

# Sensors
dht_sensor = dht.DHT11(Pin(5))   # GP5
soil = ADC(Pin(26))              # GP26

# Relay (NO wiring → normal logic)
relay = Pin(6, Pin.OUT)
relay.value(1)   # OFF initially

# Calibration values
DRY = 60000
WET = 15000

while True:
    try:
        # Read DHT
        dht_sensor.measure()
        temp = dht_sensor.temperature()
        hum = dht_sensor.humidity()

        # Read soil moisture
        soil_value = soil.read_u16()
        moisture = (DRY - soil_value) * 100 / (DRY - WET)
        moisture = max(0, min(100, moisture))

        print("Temp:", temp, "°C")
        print("Humidity:", hum, "%")
        print("Moisture:", int(moisture), "%")

        # 🌱 CONDITION
        if moisture < 30 and hum < 89:
            print("Condition met → Pump ON 💧")
            relay.value(0)   # ON

            time.sleep(10)   # pump runs (adjust if needed)

            relay.value(1)   # OFF
            print("Pump OFF")

            time.sleep(10)   # wait before next cycle

        else:
            print("Condition not met → Pump OFF")
            relay.value(1)

        print("----------------------")

    except Exception as e:
        print("Error:", e)

    time.sleep(2)
