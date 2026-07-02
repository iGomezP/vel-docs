# Epson L4360 Printer Service Runbook

## Document Control

| Field | Value |
|------|-------|
| Document Type | Runbook |
| Service | Epson L4360 Printer Service |
| Environment | Vakzor Enterprise Lab / Home Network |
| Status | Active |
| Owner | Ignacio Gomez |
| Last Updated | 2026-07-01 |

---

## Privacy and Redaction Policy

This runbook intentionally avoids exposing exact internal IP addresses, router models, hardware serial numbers, and sensitive network identifiers.

Use the placeholders below and replace them with local values only during private operations.

| Placeholder | Meaning |
|------------|---------|
| `<FEDORA_CUPS_IP>` | IP address of the Fedora CUPS server |
| `<LAB_GATEWAY_IP>` | IP address of the lab gateway reachable from the home network |
| `<HOME_NETWORK_CIDR>` | Home network CIDR |
| `<LAB_NETWORK_CIDR>` | Lab network CIDR |
| `<HOME_ROUTER>` | Home router or main home gateway |
| `<LAB_GATEWAY>` | Lab router or lab gateway |
| `<PRINTER_USB_URI>` | CUPS USB device URI for the Epson printer |
| `<EPSON_DRIVER_RPM>` | Official Epson Linux RPM package |
| `<EPSON_DRIVER_PACKAGE>` | Installed Epson driver package name |
| `<EPSON_L4360_PPD>` | Epson L4360 CUPS PPD path |

Private values must not be committed to Git.

---

## 1. Purpose

This runbook documents the final working configuration for the Epson L4360 printer service used in Vakzor Enterprise Lab.

The printer is used both as a family printer and as a lab-managed printing service through Fedora Server and CUPS.

The goal is to provide:

- Reliable home printing.
- Epson mobile app support.
- Scanning support through Epson WiFi/app.
- A stable USB fallback path through Fedora.
- Windows printing through Fedora CUPS.
- Operational documentation for future recovery.

---

## 2. Final Architecture

### 2.1 Primary Family Path

```text
Phones / Windows Devices
        ↓ WiFi
Epson L4360
```

This path is used for normal home usage.

It supports:

- Epson mobile app.
- Scanning.
- Ink level checks.
- Printer maintenance.
- Wake from app.
- Direct WiFi printing.

### 2.2 Fallback / Lab Managed Path

```text
Windows Devices
        ↓ Network / IPP
Fedora Server - CUPS
        ↓ USB
Epson L4360
```

This path is used as a stable fallback and as part of the VEL lab infrastructure.

### 2.3 Cross-Network Laptop Path

```text
Laptop
        ↓ Home WiFi / Home Network
<LAB_GATEWAY_IP>:631
        ↓ Controlled Port Forward
<FEDORA_CUPS_IP>:631
        ↓ USB
Epson L4360
```

---

## 3. Network Topology

```text
Home Network
<HOME_NETWORK_CIDR>

<HOME_ROUTER>
        │
        ├── Phones
        ├── Windows Devices
        ├── Epson L4360 WiFi
        │
        └── <LAB_GATEWAY>
            <LAB_GATEWAY_IP>
                │
                ▼
Lab Network
<LAB_NETWORK_CIDR>

Fedora Server
<FEDORA_CUPS_IP>
        │
        ▼
USB
Epson L4360
```

---

## 4. Devices

| Device | Role | Connection |
|--------|------|------------|
| Epson L4360 | Printer / Scanner | WiFi + USB |
| Fedora Server | CUPS Print Server | `<FEDORA_CUPS_IP>` |
| Lab Gateway | Home-to-lab gateway / controlled forwarding | `<LAB_GATEWAY_IP>` |
| Home Router | Home network gateway | `<HOME_NETWORK_CIDR>` |
| Windows PC | Home client | WiFi / LAN |
| Laptop | Home client | Home WiFi |
| Phones | Family printing / scanning | Epson WiFi |

---

## 5. Final Working Paths

| Source | Path | Status |
|--------|------|--------|
| Phones | WiFi direct to Epson | Working |
| Epson App | WiFi direct to Epson | Working |
| Fedora Server | USB direct to Epson | Working |
| Windows PC | Fedora CUPS over network | Working |
| Laptop | Lab gateway forwarding to Fedora CUPS | Working |

---

## 6. Original Problem

The Epson L4360 was initially detected by Fedora and CUPS using a driverless configuration.

The generated PPD showed:

```text
EPSON Printer, driverless, 2.1.1
```

When printing through this queue, the printer produced unreadable characters and multiple pages of garbage output.

CUPS also disabled the printer after backend errors.

Observed symptoms:

```text
Printer stopped due to backend errors
printer-state=5(stopped)
printer-state-reasons=paused
```

---

## 7. Root Cause

CUPS configured the printer using a driverless queue that was not correctly interpreted by the Epson L4360 over USB.

The printer required the official Epson ESC/P-R2 driver.

Incorrect path:

```text
Application
        ↓
CUPS Driverless Queue
        ↓
USB Backend
        ↓
Epson L4360 receives invalid print data
```

Correct path:

```text
Application
        ↓
CUPS
        ↓
Official Epson ESC/P-R2 Filter
        ↓
USB Backend
        ↓
Epson L4360
```

---

## 8. Official Driver

The official Epson Linux RPM driver was downloaded from Epson support.

Driver selected:

```text
Epson Inkjet Printer Driver 2 (ESC/P-R) for Linux
```

RPM package placeholder:

```text
<EPSON_DRIVER_RPM>
```

Installed package placeholder:

```text
<EPSON_DRIVER_PACKAGE>
```

Example package family:

```text
epson-inkjet-printer-escpr2
```

---

## 9. Driver Installation

Copy the RPM from Windows to Fedora Server:

```powershell
scp .\<EPSON_DRIVER_RPM> <FEDORA_USER>@<FEDORA_CUPS_IP>:/home/<FEDORA_USER>/Downloads/
```

Install on Fedora:

```bash
cd ~/Downloads
sudo dnf install ./<EPSON_DRIVER_RPM>
```

Validate installation:

```bash
rpm -qa | grep -i -E "epson|escpr"
```

Expected result:

```text
<EPSON_DRIVER_PACKAGE>
```

---

## 10. Correct CUPS Driver

Find the Epson L4360 PPD:

```bash
lpinfo -m | grep -i "L4360"
```

Expected result should include an Epson L4360 PPD from the official Epson ESC/P-R2 driver.

Correct PPD placeholder:

```text
<EPSON_L4360_PPD>
```

Do not use:

```text
driverless:ipp://EPSON...
```

---

## 11. Recreate CUPS Printer Queue

Remove the old driverless queue:

```bash
cancel -a Epson-L4360
sudo cupsdisable Epson-L4360
sudo lpadmin -x Epson-L4360
```

Validate that no printer exists:

```bash
lpstat -p -d
```

Find the USB URI:

```bash
lpinfo -v | grep -i Epson
```

Create the queue using the official Epson driver:

```bash
sudo lpadmin -p Epson-L4360 -E \
  -v "<PRINTER_USB_URI>" \
  -m "<EPSON_L4360_PPD>"
```

Set as default:

```bash
sudo lpadmin -d Epson-L4360
```

Validate:

```bash
lpstat -t
```

Expected status:

```text
scheduler is running
system default destination: Epson-L4360
device for Epson-L4360: <PRINTER_USB_URI>
Epson-L4360 accepting requests
printer Epson-L4360 is idle
```

---

## 12. Driver Validation

Check the active PPD:

```bash
sudo grep -E "NickName|ModelName|Manufacturer|cupsFilter|Product" /etc/cups/ppd/Epson-L4360.ppd
```

Expected output should include:

```text
Epson L4360 Series
epson-inkjet-printer-escpr2
epson-escpr-wrapper2
```

Bad output:

```text
driverless
```

---

## 13. Final Printer Defaults

The printer is configured for Mexico home usage:

| Setting | Value |
|---------|-------|
| Paper Size | Letter |
| Print Quality | Plain Paper High |
| Ink Mode | Color |
| Duplex | Disabled |
| Quiet Mode | Off |

Apply defaults:

```bash
sudo lpadmin -p Epson-L4360 -o PageSize=Letter
sudo lpadmin -p Epson-L4360 -o MediaType=PLAIN_HIGH
sudo lpadmin -p Epson-L4360 -o Ink=COLOR
sudo lpadmin -p Epson-L4360 -o Duplex=None
sudo lpadmin -p Epson-L4360 -o QuietMode=Off
```

Validate:

```bash
lpoptions -p Epson-L4360 -l
```

Expected active values are marked with `*`:

```text
MediaType/Print Quality: *PLAIN_HIGH ...
Ink/Grayscale: *COLOR MONO
PageSize/Media Size: ... *Letter ...
Duplex/Duplex Tumble: *None ...
QuietMode/Quiet Mode: *Off ...
```

---

## 14. Print Validation

### 14.1 Text Test

```bash
printf "VEL printer test\nEpson L4360 official driver\n" | lp -d Epson-L4360 -o PageSize=Letter
```

Expected result:

```text
One readable printed page.
```

### 14.2 Color Test

Create test file:

```bash
cat > /tmp/vel-color-test.ps <<'POSTSCRIPT'
%!PS-Adobe-3.0
%%Pages: 1
%%BoundingBox: 0 0 612 792
%%EndComments

/Helvetica-Bold findfont 24 scalefont setfont
72 720 moveto
0 0 0 setrgbcolor
(VEL Color Printer Test) show

/Helvetica findfont 16 scalefont setfont
72 680 moveto
0 0 0 setrgbcolor
(Epson L4360 - Official ESC/P-R2 Driver) show

72 630 moveto
1 0 0 setrgbcolor
(Red / Rojo) show

72 600 moveto
0 0.5 0 setrgbcolor
(Green / Verde) show

72 570 moveto
0 0 1 setrgbcolor
(Blue / Azul) show

72 540 moveto
1 0.5 0 setrgbcolor
(Orange / Naranja) show

72 510 moveto
0.5 0 0.8 setrgbcolor
(Purple / Morado) show

showpage
POSTSCRIPT
```

Print:

```bash
lp -d Epson-L4360 -o PageSize=Letter -o Ink=COLOR -o MediaType=PLAIN_HIGH /tmp/vel-color-test.ps
```

Expected result:

```text
One readable color printed page.
```

---

## 15. CUPS Sharing

Enable CUPS printer sharing:

```bash
sudo cupsctl --share-printers --remote-any
sudo lpadmin -p Epson-L4360 -o printer-is-shared=true
```

Open firewall services:

```bash
sudo firewall-cmd --add-service=ipp --permanent
sudo firewall-cmd --add-service=mdns --permanent
sudo firewall-cmd --reload
```

Validate CUPS listener:

```bash
ss -ltnp | grep 631
```

Expected result:

```text
LISTEN 0 4096 0.0.0.0:631
LISTEN 0 4096 [::]:631
```

Validate firewall:

```bash
sudo firewall-cmd --list-services
```

Expected result should include:

```text
ipp mdns
```

Validate CUPS settings:

```bash
sudo cupsctl
```

Expected values:

```text
_remote_any=1
_share_printers=1
WebInterface=Yes
```

---

## 16. Windows Configuration

### 16.1 Same Lab Network

For Windows devices that can reach the Fedora server directly:

```text
http://<FEDORA_CUPS_IP>:631/printers/Epson-L4360
```

Recommended Windows printer name:

```text
Epson L4360 - Fedora USB
```

### 16.2 Home Network Through Lab Gateway

For Windows devices in the home network that cannot reach Fedora directly:

```text
http://<LAB_GATEWAY_IP>:631/printers/Epson-L4360
```

Recommended Windows printer name:

```text
Epson L4360 - Fedora USB
```

### 16.3 Driver Selection

When Windows asks for a driver, use:

```text
EPSON L4360 Series
```

Avoid generic XPS, PCL, or PS drivers.

---

## 17. Lab Gateway Port Forwarding

The lab gateway forwards IPP traffic from the home network to Fedora CUPS.

| Field | Value |
|------|-------|
| Name | CUPS_IPP_Fedora |
| Interface | WAN / Home-facing interface |
| External Port | 631 |
| Internal Port | 631 |
| Internal Server IP | `<FEDORA_CUPS_IP>` |
| Protocol | TCP |
| Status | Enabled |

Client URL from the home network:

```text
http://<LAB_GATEWAY_IP>:631/printers/Epson-L4360
```

---

## 18. Phone Printing Decision

Phones should use the Epson WiFi direct path instead of Fedora CUPS.

Final decision:

```text
Phones → Epson WiFi
Windows → Epson WiFi or Fedora USB fallback
Fedora → USB direct
```

Reason:

- Epson app supports scanning.
- Epson app supports printer maintenance.
- Epson app supports ink level monitoring.
- Epson app supports waking the printer.
- Mopria did not accept the Fedora CUPS IPP/HTTP URL.
- CUPS sharing works better for Windows than Android in this setup.

---

## 19. Common Operations

### Check Printer Status

```bash
lpstat -t
```

### Cancel All Jobs

```bash
cancel -a Epson-L4360
```

### Disable Printer

```bash
sudo cupsdisable Epson-L4360
```

### Enable Printer

```bash
sudo cupsenable Epson-L4360
sudo cupsaccept Epson-L4360
```

### Restart CUPS

```bash
sudo systemctl restart cups
```

### Check CUPS Service

```bash
systemctl status cups --no-pager
```

### Check CUPS Logs

Fedora may use journald instead of `/var/log/cups/error_log`.

```bash
sudo journalctl -u cups -n 100 --no-pager
```

---

## 20. Troubleshooting

### 20.1 Printer Prints Garbage Characters

Likely cause:

```text
Wrong driver or driverless queue.
```

Validate driver:

```bash
sudo grep -E "NickName|cupsFilter|Product" /etc/cups/ppd/Epson-L4360.ppd
```

If output contains `driverless`, recreate the queue using the official Epson ESC/P-R2 driver.

---

### 20.2 Printer Is Disabled

Enable it:

```bash
sudo cupsenable Epson-L4360
sudo cupsaccept Epson-L4360
```

Check status:

```bash
lpstat -t
```

---

### 20.3 Windows Cannot Open Fedora CUPS URL

Check whether Windows is on the lab network or home network.

Use the direct Fedora URL only when the device can reach `<FEDORA_CUPS_IP>`:

```text
http://<FEDORA_CUPS_IP>:631/printers/Epson-L4360
```

Use the lab gateway URL when the device is on the home network:

```text
http://<LAB_GATEWAY_IP>:631/printers/Epson-L4360
```

---

### 20.4 Laptop Cannot Reach Fedora CUPS Directly

This is expected if the laptop is on the home network and the lab network is separated behind a gateway.

Use the forwarded lab gateway URL:

```text
http://<LAB_GATEWAY_IP>:631/printers/Epson-L4360
```

---

### 20.5 Android / Mopria Does Not Accept CUPS URL

Use Epson WiFi direct printing from the phone.

Fedora CUPS is used as a Windows/fallback print path, not as the primary phone print path.

---

## 21. Security Notes

CUPS must not be exposed directly to the Internet.

Allowed use:

```text
Home Network → Lab Gateway → Fedora CUPS
```

Not allowed:

```text
Internet → CUPS
```

The home router should not expose CUPS publicly through WAN port forwarding.

The lab gateway forwarding rule is used only inside the home/lab network path.

---

## 22. Final Status

| Item | Status |
|------|--------|
| Fedora detects Epson L4360 over USB | Complete |
| Official Epson ESC/P-R2 driver installed | Complete |
| Driverless queue removed | Complete |
| Official Epson CUPS queue created | Complete |
| Text printing validated | Complete |
| Color printing validated | Complete |
| Letter paper configured | Complete |
| High quality configured | Complete |
| CUPS sharing enabled | Complete |
| Firewall allows IPP/mDNS | Complete |
| Windows personal PC prints through Fedora | Complete |
| Laptop prints through lab gateway forwarding | Complete |
| Phones use Epson WiFi path | Complete |
| Scanning preserved through Epson WiFi/app | Complete |

---

## 23. Lessons Learned

- Driverless printing is convenient but not always reliable.
- Official printer drivers are still required for some USB printer workflows.
- CUPS sharing works well for Windows clients.
- Phone printing should stay on the vendor WiFi/app path when scanning and maintenance features matter.
- Home and lab network separation is useful, but cross-network services need routing or controlled forwarding.
- Operational fixes should be documented as runbooks.
