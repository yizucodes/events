# Give the Body a Brain

**Turn ReSpeaker Flex into a Baymax-like AI companion that listens, thinks, speaks, and acts.**

In this Robotics Fair 2026 workshop with Seeed Studio, you will connect a physical voice device to conversational intelligence. By the end, your ReSpeaker Flex will be paired with an AI character in Bot Station and ready to listen, think, speak, and act through tool integrations.

The experience comes together in three layers:

1. **Persona:** define your AI character.
2. **Voice:** give the character the ability to listen and speak.
3. **Body:** run the experience on real ReSpeaker hardware.

## Workshop Flow

| Section | Focus | Outcome |
| --- | --- | --- |
| 1 | Introduction | Understand how ReSpeaker Flex gives an AI character a physical voice and presence. |
| 2 | Preparations | Confirm that your account, ReSpeaker Flex, and USB cable are ready. |
| 3 | Meet ReSpeaker Flex | Learn the hardware architecture and main controls. |
| 4 | Character setup | Create and preview your AI character in Bot Station. |
| 5 | Flash, pair, and run | Flash ReSpeaker Flex, connect it to your bot, and activate it. |
| 6 | Explore more | Extend the project with source code, voice features, and MCP tools. |

## 1. Give the Body a Brain

Links:

- [Agora](https://www.agora.io/en/)
- [Seeed Studio](https://www.seeedstudio.com/)
- [Robotics Fair 2026](https://robotfaire.org/)

## 2. Prepare Your Workshop Kit

Make sure the account, ReSpeaker Flex, and cable are ready before the live setup begins.

- Computer with internet access
- One ReSpeaker Flex with XIAO ESP32-S3, confirm speaker and 4-mic array are connected
- USB cable for device flashing
- [Agora account](https://console.agora.io/), [Sign Up](https://sso2.agora.io/en/signup) if you don't have it
- Create a project and make sure "Conversational AI Engine" is enabled(enabled by default)
- Access to [Bot Station](https://botstation.sg3.agoralab.co/)

### Optional: Prepare Debugging Tools

Use [`tio`](https://github.com/tio/tio) on macOS or Linux, or PuTTY on Windows, to monitor live serial logs from ReSpeaker Flex.

**macOS:**

```bash
brew install tio
ls /dev/cu.*
```

**Windows:** install [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html):

```powershell
winget install PuTTY.PuTTY
```

Open PuTTY, select **Serial**, choose the device's `COM` port from Device Manager, set **Speed** to `115200`, and select **Open**.

**Linux:**

```bash
sudo apt update && sudo apt install tio
ls /dev/ttyACM* /dev/ttyUSB*
```

On macOS or Linux, connect to the serial port and monitor logs:

```bash
tio /dev/cu.usbmodem1101
```

Replace `/dev/cu.usbmodem1101` with your device port. Linux commonly uses `/dev/ttyACM0` or `/dev/ttyUSB0`. On Windows, replace `COM3` with the port shown in Device Manager.

Agora Physical AI connects AI systems to real-world devices so hardware can listen, speak, understand, and act. Seeed Studio brings ReSpeaker Flex, XIAO ESP32-S3, and the XVF3800 circular four-microphone array together for robotics and embodied AI.

## 3. Meet ReSpeaker Flex

ReSpeaker Flex is the physical interface for your AI character. It provides Wi-Fi connectivity, far-field voice input, speaker output, and the controls needed for flashing and pairing.


### Hardware Architecture

![ReSpeaker hardware architecture](3-respeaker-hardware-architecture.jpg)

### Main Controls

- **Activate[RESET]:** start or stop the conversation after pairing.
- **Pair[BOOT]:** hold to reconfigure Wi-Fi; press after Wi-Fi connects to start pairing.

![ReSpeaker Flex with four-microphone array](3-respeaker-flex-xvf3800-circular-4_1_.jpg)


## 4. Create Your AI Character

Use Bot Station to define how your physical AI behaves, speaks, and greets users.

### Open Bot Station

1. Visit [Bot Station](https://botstation.sg3.agoralab.co/).

![Bot Station main page](4-botstation-main.png)

### Create a New Bot

1. Create a new bot.
2. Select the Agora project you want to use.
3. Continue to character setup.

![Create a new bot](4-botstation-add-new-bot.png)

### Configure the Character

- **Bot name:** choose a recognizable name, such as `Workshop Companion`.
- **Persona:** describe the character, expertise, and tone.
- **Language:** choose the conversation language.
- **Voice:** select an available text-to-speech voice.
- **System prompt:** define behavior, boundaries, and knowledge scope.
- **Welcome message:** write the first message spoken after activation.
- **Filler words(Optional):** add short phrases for moments when a response is still being generated.
- **MCP server(Optional):** optionally connect tools that can retrieve data or perform actions.

Example system prompt:

```text
You are a friendly workshop assistant running on a small voice device. Keep answers short, practical, and easy to understand. Explain the next workshop step clearly when asked.
```

![Config the created bot](4-botstation-add-new-bot-config.png)

### Save and Preview

1. Save the bot.
2. Open Preview.
3. Test a short conversation in the web client.
4. Confirm that the persona, language, and voice sound correct.

Try these prompts:

```text
Hello, who are you?
What can you help me with?
Explain this workshop in one sentence.
```

![Try it](4-botstation-add-new-bot-try-without-hardware.png)

## 5. Flash, Pair, and Run

Open your bot in **My Bots**, choose **Add device**, pick and download the firmware for reSpeaker(reSpeaker-flex-esp32s3), then select one of the two flashing workflows.

### Option A: Web Flasher

The [web flash tool](https://thelastoutpostworkshop.github.io/ESPConnect/) runs in your browser. It does not require an app download or installation.

![Connect device to flashtool](5-flashtool-web-choose-device.png)

![Flash with web tool](5-flashtool-web-flash.png)

### Option B: esptool

Install Espressif [esptool](https://docs.espressif.com/projects/esptool/en/latest/esp32/) for a local flashing workflow.

Update the serial port and firmware image path, then run:

```bash
esptool -p /dev/cu.wchusbserial1320 -b 460800 write_flash --erase-all 0x0 /path/to/reSpeaker-flex-esp32s3_sg3.bin
```

### Flash the Firmware

1. Connect ReSpeaker Flex to your computer with the USB cable.
2. Select the correct COM or serial port.
3. Set the baud rate to `460800`.
4. Select the ReSpeaker firmware image and start flashing.
5. Wait for the flashing process to finish successfully.

### Optional, IGNORE THIS IF YOU DON'T KNOW WHAT IT IS: Flash XVF3800 I2S Firmware

The XIAO ESP32-S3 communicates with the XVF3800 microphone array over I2S. Flash the separate I2S firmware if the device still uses its factory USB firmware or produces loud static or noise instead of clear microphone audio. Skip this update when I2S audio is already clean.

1. Connect your computer to the XMOS USB-C port near the RST button, not the XIAO USB-C port.
2. Install [`dfu-util`](http://dfu-util.sourceforge.net/): on Windows, download version 0.11, extract the `win64` directory, and add it to the system `Path`; use `brew install dfu-util` on macOS or `sudo apt install dfu-util` on Linux.
3. Run `dfu-util -l` to confirm that the XVF3800 is detected.
4. If it is not detected, hold the Boot button while reconnecting power to enter Safe Mode. On Windows, install the WinUSB driver with [Zadig](https://zadig.akeo.ie/) if `dfu-util` reports a USB access error.
5. Download [`respeaker_flex_i2s_c16k2ch_v1.0.0.bin`](https://github.com/respeaker/reSpeaker_Flex/raw/refs/heads/main/xmos_firmwares/i2s/respeaker_flex_i2s_c16k2ch_v1.0.0.bin) from the official GitHub repository and run:

```bash
dfu-util -R -e -a 1 -D /path/to/respeaker_flex_i2s_c16k2ch_v1.0.0.bin
```

See the [official ReSpeaker Flex firmware guide](https://wiki.seeedstudio.com/respeaker_flex_introduction/#update-firmware) for Windows setup and recovery details.

![Add a new device](5-botstation-add-new-device.png)

### Pair and Activate

1. Power off and on ReSpeaker Flex to start Wi-Fi configuration.
2. Connect your computer to the Wi-Fi hotspot(Name starts with Agora-Convo) created by ReSpeaker Flex.
3. Open `http://192.168.4.1/` if the configuration page does not open automatically.
4. Wait for ReSpeaker Flex to connect to Wi-Fi.
5. After Wi-Fi connects, press Boot / Pair to start pairing and listen for the spoken pairing code.
6. Return to Bot Station and enter the spoken pairing code.
7. Submit the form and wait for pairing to complete. The first conversation starts automatically after pairing.
8. Press Reset to stop or start the conversation again.

![Device pairing page](5-botstation-add-new-device-pair.png)

![Device pairing code](5-botstation-add-new-device-pair-code.png)

Suggested voice tests:

```text
Hi, introduce yourself.
What should I do next in this workshop?
Tell me one fun fact about physical AI.
```

## 6. Explore More

Once ReSpeaker Flex is working, continue with these extensions:

- **Source code:** clone and modify the [open-source ReSpeaker firmware project](https://github.com/qiuyanli1990/respeaker-flex-circle-Agora-mybot).
- **Voice locking:** explore identity-aware interactions for personalized device experiences.
- **Voice clone:** coming soon.
- **Tool actions:** connect MCP tools so the device can retrieve external data or trigger services.
- **Reache Mini + ReBot ARM:** We will provide the live demo and source code

### Free Weather MCP Sample

The [official TypeScript Weather MCP sample](https://github.com/modelcontextprotocol/quickstart-resources/tree/main/weather-server-typescript) runs locally over stdio and uses the free US National Weather Service API.

- MCP transport: `stdio: node .../weather/build/index.js`
- Weather data endpoint: `https://api.weather.gov`
- Cost: free public data
- API key: not currently required
- Available tools: `get_forecast` and `get_alerts`

Install and build the sample, then add it to your MCP client configuration:

```json
{
  "mcpServers": {
    "weather": {
      "command": "node",
      "args": ["/ABSOLUTE/PATH/weather/build/index.js"]
    }
  }
}
```

See the [official MCP server tutorial](https://modelcontextprotocol.io/docs/develop/build-server) and the [NWS API documentation](https://www.weather.gov/documentation/services-web-api) for implementation details and usage policies.

Join other voice AI builders in the Agora Discord community:

![QR code to join the Agora Discord community](join-discord-agora-qr.jpg)

[Checkout full step-by-step tutorial videos](https://drive.google.com/drive/folders/1t2uWEdmkG84dJgzPTc2KT8XzHd8HxsC5)

## Troubleshooting

| Problem | What to Check |
| --- | --- |
| Computer cannot find ReSpeaker Flex | Reconnect the USB cable, try another USB port, or confirm the serial driver is installed. |
| Flashing does not start | Confirm the serial port and `460800` baud rate, then retry the flash command. |
| Pairing page does not open | Connect to the ReSpeaker hotspot(Name starts with Agora-Convo) and manually open `http://192.168.4.1/`. |
| Cannot connect to Wi-Fi | Confirm the access point supports 2.4 GHz Wi-Fi, the antenna is connected securely, and the password is correct. |
| Pairing code fails | Get the spoken code and put into Bot Station again. |
| Bot does not respond | Confirm Wi-Fi is configured, the bot is saved, ReSpeaker Flex is activated and wait for longer time. |
| Voice sounds wrong | Recheck the selected text-to-speech voice in Bot Station. |

## Completion Checklist

- AI character is created and saved in Bot Station.
- Bot preview works in the web client.
- ReSpeaker firmware flashing completes successfully.
- ReSpeaker Flex enters pairing mode and opens the configuration page.
- Pairing code is accepted.
- Activate starts the bot session.
- The physical AI companion can listen, respond, and speak aloud.
