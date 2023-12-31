# How to make a custom systemd service

**Last updated**: October 20, 2019

Basic systemd commands:

- `systemctl status`: Show status of all systemd service
- `systemctl daemon-reload`: Reload systemd if units are modified
- `systemctl enable`: to enable a new service
- `systemctl start`: to start a new service
- `systemctl restart`: to restart a service


## Example service file

1. Make new service unit file in `/etc/systemd/system/new-service.service`:

    ```bash
    [Unit]
    Description = Sensai background service
    After = network.target

    [Service]
    ExecStart = /usr/bin/python3 /home/lento/projects/sensai/log.py
    User = lento
    Group = lento

    [Install]
    WantedBy = default.target
    ```

2. Reload systemd and enable the service

    ```
    systemctl daemon-reload
    systemctl enable new-service.service
    ```

3. Start the new service

    ```
    systemctl start new-service.service
    ```

4. Inspect the status

    ```
    systectl status new-service.service
    ```
