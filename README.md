#NVIDIA GPU Power & Clock Limiter

This repository contains a systemd service and a Bash script designed to automatically manage NVIDIA GPU power limits and clock speeds at system boot. This is particularly useful for headless servers, mining rigs, or embedded systems where thermal management and power efficiency are priorities.

#Features

Persistent Mode: Automatically enables NVIDIA Persistence Mode.

Power Capping: Sets a hard limit on GPU power consumption in Watts.

Clock Locking: Locks both Memory and GPU core clocks to a specific range to prevent frequency spikes and thermal throttling.

Automation: Integrates with systemd to ensure settings are applied after the nvidia-persistenced daemon is ready.

#Files

    set-nvidia-tdp.sh: The execution script that interfaces with nvidia-smi to apply hardware constraints.

nvidia-tdp.service: The systemd unit file that manages the script execution during the boot sequence.

#Configuration

Before installation, you can modify the following variables in set-nvidia-tdp.sh to match your specific hardware requirements:

Variable	Description	Default Value
POWER_LIMIT	Maximum power draw in Watts	25W
GPU_CLOCK_MIN	Minimum Core Clock frequency	210 MHz
GPU_CLOCK_LIMIT	Maximum Core Clock frequency	1000 MHz
MEMORY_CLOCK_MIN	Minimum Memory Clock frequency	405 MHz
MEMORY_CLOCK_LIMIT	Maximum Memory Clock frequency	5001 MHz


#Installation

1. Deploy the Script

Move the script to your local bin and ensure it is executable:

Bash

sudo cp set-nvidia-tdp.sh /usr/local/bin/set-nvidia-tdp.sh
sudo chmod +x /usr/local/bin/set-nvidia-tdp.sh

2. Deploy the Service

Move the service file to the systemd directory:

Bash

sudo cp nvidia-tdp.service /etc/systemd/system/nvidia-tdp.service

3. Enable and Start

Reload the daemon and enable the service so it runs at every boot:

Bash

sudo systemctl daemon-reload
sudo systemctl enable nvidia-tdp.service
sudo systemctl start nvidia-tdp.service

#Requirements

    NVIDIA Drivers: Must be installed and functioning.

    nvidia-smi: The command-line utility must be available at /usr/bin/nvidia-smi.

nvidia-persistenced: The service must be enabled as the TDP script relies on it.

    Note for the "Thin" Enthusiast: This setup keeps your power draw thin and your thermals cool. If the aliens finally land in Transylvania, at least your GPUs won't be the reason the grid goes down.

Would you like me to add a Python-based monitor script to log these metrics to a CSV for your IOT dashboard?
