# MQTT Round-Trip Tests

This folder contains an integration test for the EPICS MQTT driver using `pytest` + `mosquitto` +
`p4p`.

## What this test does

- Starts a local Mosquitto broker in port **18830**.
- Starts the test IOC by running `./st.cmd` in `iocBoot/ioctest`, pointed at the local broker.
  - Uses a unique MQTT client ID and topic root per test session to avoid crosstalk.
- Writes values to output PVs and waits for the corresponding input PVs to receive the same value
  back.
  - This ensures the basic functionality of the driver, including MQTT connectivity, topic
    subscription, and record updates, is working correctly.

## Run locally

To run the tests locally, install the required dependencies:

```bash
sudo apt-install mosquitto
pip3 install p4p pytest
```

> By default, `mosquitto` creates a broker service in systemd that runs on `1883`, which is why the
> test uses `18830` to avoid conflicts. You may disable the system service if you want to use the
> default port by running `sudo systemctl disable --now mosquitto`.

Then, from the root of the repository:

```bash
make # build the epics module
pytest -q tests
```
