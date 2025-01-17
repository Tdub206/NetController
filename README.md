# NetController
Pentesters Automated Tool 

Interface Preparation

bashCopy# List available interfaces
iwconfig

# Kill potentially interfering processes
sudo airmon-ng check kill

# Enable monitor mode on both interfaces
sudo airmon-ng start wlan0
sudo airmon-ng start wlan1

# Verify monitor mode
iwconfig wlan0mon
iwconfig wlan1mon

Backend Deployment

bashCopy# Set execution permissions
chmod +x backend.py

# Configure Flask environment
export FLASK_APP=backend.py
export FLASK_ENV=development

# Run backend with elevated privileges
sudo python3 backend.py

Frontend Deployment

bashCopy# Install Node.js and npm
sudo apt install -y nodejs npm

# Install dependencies
npm install axios recharts lucide-react @tailwindcss/forms

# Build and serve frontend
npm run build
npm run serve

Advanced Configuration Parameters

Create a config.json file:
jsonCopy{
  "interfaces": {
    "primary": "wlan0mon",
    "secondary": "wlan1mon"
  },
  "attack_parameters": {
    "deauth_interval": 0.1,
    "burst_count": 64,
    "channel_hop_interval": 0.5
  },
  "capture_settings": {
    "buffer_size": 65535,
    "timeout": 30
  }
}

Execute Attack Sequence

bashCopy# Terminal 1 - Backend API
sudo python3 backend.py

# Terminal 2 - Frontend Server
npm run serve

# Terminal 3 - Monitor Mode Verification
sudo tcpdump -i wlan0mon -n

Performance Optimization

Add to /etc/sysctl.conf:
bashCopy# Optimize network buffer
net.core.rmem_max=16777216
net.core.wmem_max=16777216

# Enhance UDP performance
net.ipv4.udp_mem="16777216 16777216 16777216"

Security Considerations

bashCopy# Implement strict file permissions
sudo chown root:root backend.py
sudo chmod 700 backend.py

# Configure firewall rules
sudo iptables -A INPUT -p tcp --dport 5000 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 3000 -j ACCEPT
Advanced Usage Parameters:

Precision Timing Control:

pythonCopy# Modify in backend.py
INJECTION_PRECISION = 1e-9  # Nanosecond precision
FRAME_INTERVAL = 0.1       # 100ms between frames
BURST_SIZE = 64           # Frames per burst

Channel Coordination:

pythonCopydef optimize_channel_selection(interface):
    channel_metrics = {}
    for channel in range(1, 14):
        subprocess.run(["iwconfig", interface, "channel", str(channel)])
        metrics = measure_channel_metrics()
        channel_metrics[channel] = calculate_channel_score(metrics)
    return max(channel_metrics, key=channel_metrics.get)

Memory Management:

bashCopy# Add to /etc/security/limits.conf
* soft memlock 16777216
* hard memlock 16777216
