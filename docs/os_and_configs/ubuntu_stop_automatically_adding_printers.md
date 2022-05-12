# Stop Ubuntu from Adding All Printers on Network

Open terminal and type in command:

```bash
sudo systemctl stop cups-browsed
```

followed by :

```bash
sudo systemctl disable cups-browsed
```

You may still start/stop the service manually if you want:

```bash
sudo systemctl start cups-browsed
sudo systemctl stop cups-browsed
```
