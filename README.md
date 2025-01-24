# Streamlining-IoT-File-Transfers-Setting-Up-an-FTP-Server-for-WyzeCam-v2-and-ESP32

# Setting Up an FTP Server and Creating a Local Directory

## A Step-by-Step Guide

### Introduction
Setting up an FTP (File Transfer Protocol) server allows you to transfer files between computers over a network. This guide will walk you through setting up an FTP server and configuring a local directory on your computer.

---

## Prerequisites
Before starting, ensure you have:
- A computer with administrative privileges
- Stable internet connection
- FTP server software (e.g., FileZilla Server)

---

## Step 1: Installing FTP Server Software

### Windows (Using FileZilla Server)
1. Download the FileZilla Server installer from the [official website](https://filezilla-project.org/).
2. Run the installation and follow the instructions.
3. Launch FileZilla Server Interface after installation.

### Configuring FileZilla Server
1. Open the FileZilla Server Interface.
2. Go to **Edit > Settings**.
3. Configure general settings, such as listening port and security options.
4. Add users and assign them directories by going to **Edit > User**.

---

## Step 2: Setting Up a Local Directory

### FileZilla Server
1. In the FileZilla Server Interface, go to **Edit > User**.
2. Select a user or create a new one.
3. Under the **Shared folders** section, click **Add** to specify the local directory you want to share.
4. Set permissions for the directory (e.g., read, write).

---

## Step 3: Connecting to the FTP Server
Use an FTP client (e.g., FileZilla Client, WinSCP) to connect to your FTP server.  
Enter the server’s IP address, username, and password to access the shared directories.

---

## Advanced Configuration: FTP for IoT Devices

### Setting Up FTP on WyzeCam v2

1. **Edit `motion.conf` Configuration:**
   - Navigate to `/config/motion.conf` on the WyzeCam's SD card or via SSH.
   - Update snapshot and FTP settings:
     ```bash
     # Snapshots
     save_snapshot=true

     # Configure FTP snapshots and videos
     ftp_snapshot=true
     ftp_host="192.168.10.100"
     ftp_port=21
     ftp_username="user1"
     ftp_password="********"
     ftp_stills_dir="/stills"
     ```

2. **Directory Setup:**
   - Ensure your FTP server’s virtual and native paths match the settings. Example:
     ```
     Virtual Path: /
     Native Path: C:/user/my-user/desktop/ftp/user1
     ```

---

### Setting Up FTP on ESP32

1. **Install Required Libraries in Arduino IDE:**
   - Search and install the `FTP ESP32` libraries by Leonardo Bispo using the Library Manager.

2. **Program the ESP32:**
   - Example sketch:
     ```cpp
     #include "esp_camera.h"
     #include <WiFi.h>
     #include <ESP32_FTPClient.h>

     const char* ssid = "YourSSID";
     const char* password = "YourPassword";

     char ftp_server[] = "192.168.10.100";
     char ftp_user[] = "ESP32-1";
     char ftp_pass[] = "YourPassword";

     ESP32_FTPClient ftp(ftp_server, ftp_user, ftp_pass);

     void setup() {
         Serial.begin(115200);
         WiFi.begin(ssid, password);
         while (WiFi.status() != WL_CONNECTED) {
             delay(500);
             Serial.print(".");
         }
         Serial.println("WiFi connected");

         ftp.OpenConnection();
         takeAndUploadPicture();
     }

     void takeAndUploadPicture() {
         // Capture and upload image to FTP
         Serial.println("Picture uploaded");
     }

     void loop() {
         delay(60000); // Upload every 60 seconds
     }
     ```

---

## Conclusion
This guide demonstrates how to configure an FTP server for local file transfers and integrate IoT devices, such as WyzeCam and ESP32, for automated image uploads. Customize the configurations to fit your use case and ensure optimal security settings for production environments.

---
