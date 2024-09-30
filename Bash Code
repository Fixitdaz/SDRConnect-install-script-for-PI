# PowerShell Script to Install SDRconnect on Remote Raspberry Pi

# Replace with your Raspberry Pi's IP address
$piAddress = "raspberry.local"
$piUsername = "pi"

# Ensure you have SSH access set up to your Raspberry Pi

# The bash script content (same as above, but as a here-string in PowerShell)
$bashScript = @'
#!/bin/bash
set -e
echo "Updating system packages..."
sudo apt update && sudo apt upgrade -y
echo "Installing dependencies..."
sudo apt install -y wget
echo "Downloading SDRconnect..."
wget https://www.sdrplay.com/downloads/SDRconnect/SDRconnect_installer_rpi.sh
chmod +x SDRconnect_installer_rpi.sh
echo "Installing SDRconnect..."
./SDRconnect_installer_rpi.sh
echo "Creating SDRconnect service..."
sudo tee /etc/systemd/system/sdrconnect.service > /dev/null <<EOT
[Unit]
Description=SDRconnect Server
After=network.target

[Service]
ExecStart=/opt/sdrconnect/SDRconnect --server
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
EOT
sudo systemctl daemon-reload
sudo systemctl enable sdrconnect.service
echo "Starting SDRconnect service..."
sudo systemctl start sdrconnect.service
echo "SDRconnect installation and service setup complete!"
echo "You can check the status of the service with: sudo systemctl status sdrconnect.service"
'@

# Save the bash script to a file
$bashScript | Out-File -Encoding ASCII install_sdrconnect.sh

# Copy the script to the Raspberry Pi
scp install_sdrconnect.sh ${piUsername}@${piAddress}:~/install_sdrconnect.sh

# Execute the script on the Raspberry Pi
ssh ${piUsername}@${piAddress} "chmod +x ~/install_sdrconnect.sh && sudo ~/install_sdrconnect.sh"

# Clean up the local script file
Remove-Item install_sdrconnect.sh

Write-Host "SDRconnect installation on Raspberry Pi complete!"
