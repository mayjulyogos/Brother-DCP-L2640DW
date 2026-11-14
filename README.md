# Brother-DCP-L2640DW
This is a repo which includes the driver files and folder. Here we got two ways to install the printer driver using arch linux which is the automated YAY and manually install.

<br>

## Install CUPS and Dependencies
```bash
sudo pacman -S cups usbutils
```

<br>

## Automated Installation using YAY AUR Helper
```bash
# 1. Update your system and install build tools & required dependencies
sudo pacman -Syu --needed base-devel git lib32-glibc poppler-data cups-filters

# 2. Ensure the CUPS printing service is enabled and running
sudo systemctl enable --now cups.service

# 3. Install the official AUR driver package using yay
yay -S brother-dcpl2640dw

# 4. Check for your Device URI
lpinfo -v

# 5. Register the printer queue in CUPS using your exact USB serial URI and the AUR PPD file
sudo lpadmin -p DCPL2640DW -E -v usb://Brother/DCP-L2640DW?serial=E82897F6N254111 -P /usr/share/cups/model/Brother/brother-DCPL2640DW-cups-en.ppd

# 6. Restart the CUPS print scheduler to load everything properly
sudo systemctl restart cups.service

# To print file using command line
lp -d DCPL2640DW -o sides=two-sided-long-edge -o fit-to-page file.pdf
```

<br>

## Manually install the driver from extracted driver folder
```bash
# 1. Update system and install CUPS + required 32-bit & PDF dependencies
sudo pacman -Syu --needed cups cups-filters lib32-glibc poppler-data

# 2. Enable and start the CUPS printing service
sudo systemctl enable --now cups.service

# 3. Install the driver manually from your extracted Brother RPM/DEB data folder
sudo mkdir -p /usr/share/cups/model/Brother
sudo cp brother-DCPL2640DW-cups-en.ppd /usr/share/cups/model/Brother/
sudo cp brother_lpdwrapper_DCPL2640DW /usr/lib/cups/filter/
sudo chmod +x /usr/lib/cups/filter/brother_lpdwrapper_DCPL2640DW
sudo cp -r brother /opt/

# 4. Verify your USB Device URI
lpinfo -v

# 5. Register the printer queue in CUPS
sudo lpadmin -p DCPL2640DW -E -v usb://Brother/DCP-L2640DW?serial=E82897F6N254111 -P /usr/share/cups/model/Brother/brother-DCPL2640DW-cups-en.ppd

# 6. Restart CUPS to load the new queue and filters
sudo systemctl restart cups.service

# Print a duplex, fit-to-page PDF from the command line
lp -d DCPL2640DW -o sides=two-sided-long-edge -o fit-to-page file.pdf
```
