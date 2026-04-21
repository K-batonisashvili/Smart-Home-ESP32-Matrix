# Smart Home Transit & Weather Matrix

A custom physical digital display built to show real-time public transit schedules, localized weather data, and timekeeping. 
My goal was always to create a fun, smart display which reduces my smartphone dependency and adds to my home decor! 

> **Acknowledgments:** This project was heavily inspired by and builds upon the open-source [Transit Tracker](https://github.com/EastsideUrbanism/transit-tracker) by **Eastside Urbanism**. Their foundational work, detailed instructions, and open-source framwork made this customized build possible!

> **Note:** This repository documents my personal implementation, custom logic, and physical build architecture. It is not intended as a step-by-step DIY tutorial, but rather a showcase of hardware/software integration.

---

## Features
* **Live Transit Polling:** Tracks my preferred local bus & train routes in real-time, displaying accurate arrival countdowns along with estimated travel time to the stops.
* **Custom Weather & Time Engine:** Integrates self-programmed weather polling and a live clock alongside the transit data, giving me a centralized location to get all my info as I'm making my morning cup of coffee.
* **Physical Enclosure:** 3D printed enclosure which slots the displays and microcontrollers into a tightly screwed case.
* **Automation:** Programmed the display to follow sun schedule for dynamic brightness adjustment, automatically restart the device for maintenance, and created checkpoints for route confirmation to ensure schedule accuracy.

---

## Tech Stack
**Software & Data**
* Local Transit API (real-time bus telemetry)
* Weather API (current conditions and temperature)
* ESPHome (Microcontroller Firmware & Automation logic)
* YAML / C++ (Custom display rendering and data parsing)

**Hardware**
* ESP32 Microcontroller (Adafruit Matrix Portal S3)
* 64x32 RGB LED Matrix
* Custom physical enclosure/case

---

## Architecture Overview
This system operates as a standalone, self-updating IoT appliance. This does not rely on any proprietary software, it's completely open-source, fully customizable, and consumes barely any energy since it's entirely standalone.
* **Data Pipeline:** The microcontroller independently pings the transit and weather APIs via Wi-Fi at set intervals. It parses the JSON payloads locally to extract only my specific stop IDs, route numbers, and weather metrics.
* **Display Rendering:** Using ESPHome's low-level display engine, the parsed data is translated into strict X/Y pixel coordinates and pushed to the HUB75 LED matrix. 
* **Automation Layer:** The firmware exposes its internal states to my local Wi-Fi network, allowing external home automation systems to dynamically trigger screen-wake, brightness, data logging, route confirmation, and much more.

---

## Challenges & Implementation
* **API Filtering on Microcontrollers:** Public transit APIs often return massive payloads containing every active bus in the city. A major implementation focus was writing lightweight parsing logic to filter the noise and isolate my specific routes without overflowing the ESP32's limited memory.
* **Pixel-Perfect UI:** Designing for a 64x32 pixel canvas requires strict spatial management. I had to custom-program the layout to ensure the weather icons, time, and scrolling bus arrivals all remained highly legible from across the room without overlapping.
* **Hardware Protection:** Bare circuit boards are fragile and prone to short circuits. The addition of the physical case not only secured the heavy power wiring but included wall mounting points, microcontroller slots for position-agnostic installation, and internal hooks for velcro straps.
