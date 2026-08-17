# Evidence Screenshot Guide

Student ID: 23127081

Full name: Nguyễn Phan Hùng Linh

This guide explains how to capture the missing hardware and live resource
evidence. Save every final screenshot in the `images/` folder.

## 1. Hardware screenshot with screenfetch

1. Open a new Terminal window.
2. Make the window large enough to show every line.
3. Run:

   ```sh
   screenfetch
   ```

4. Check that the output shows the computer model, operating system, CPU, and
   memory.
5. Capture only that Terminal window:

   ```sh
   screencapture -i images/hardware_screenfetch.png
   ```

6. Drag over the complete Terminal window and press Return.
7. Open `images/hardware_screenfetch.png` and confirm that no line is cut off.

## 2. Prepare htop for a test screenshot

1. Start the EShop backend.
2. Confirm that the backend PID exists:

   ```sh
   pgrep -f 'node server.js'
   ```

3. Open a second Terminal window.
4. Run `htop` with only the backend PID:

   ```sh
   htop -p "$(pgrep -f 'node server.js' | tail -n 1)"
   ```

5. Press `t` if needed to show the process tree.
6. Resize this Terminal so the PID, CPU%, MEM%, command, and memory columns are
   visible.

## 3. Capture Load evidence

1. Put the JMeter CLI Terminal on the left and the `htop` Terminal on the
   right.
2. Start the Load command from the main report.
3. Wait until most users have started and `htop` shows changing CPU values.
4. Run this from a third Terminal or use macOS `Shift + Command + 4`:

   ```sh
   screencapture -i images/load_cli_htop.png
   ```

5. Capture both terminals in one image.
6. Check that the JMeter plan name and the backend process are readable.

## 4. Capture Stress evidence

Repeat the same steps with the Stress command. Capture during the 30-user
period and save the file as:

```sh
screencapture -i images/stress_cli_htop.png
```

## 5. Capture Spike evidence

1. Start the high-load Spike command with 30 users.
2. Wait until the JMeter summary shows active requests.
3. Confirm that `htop` still shows the backend process.
4. Save the screenshot as:

   ```sh
   screencapture -i images/spike_cli_htop.png
   ```

## 6. Capture Endurance evidence

1. Start the 100-user endurance command.
2. Wait until the ramp-up is complete.
3. Capture JMeter CLI and `htop` together:

   ```sh
   screencapture -i images/endurance_cli_htop.png
   ```

## 7. Final screenshot check

Confirm that these five files exist:

- `images/hardware_screenfetch.png`
- `images/load_cli_htop.png`
- `images/stress_cli_htop.png`
- `images/spike_cli_htop.png`
- `images/endurance_cli_htop.png`

Each test image must show the JMeter CLI output and live backend resource use
in the same frame. Do not include passwords, tokens, or private data.
