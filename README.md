# WOL Fire TV - Wake-on-LAN for Amazon Fire TV Stick 4K

A complete Android application that runs on Amazon Fire TV Stick 4K to provide Wake-on-LAN functionality via HTTP API with GUI and web-based management interface. Features secure authentication, configurable access control, and seamless web interface integration.

## 🚀 Features

- ✅ **Background Service**: Runs as a foreground service with persistent notification
- ✅ **HTTP Server**: Embedded NanoHTTPD server with REST API  
- ✅ **Web Interface**: Password-protected web UI with seamless authentication
- ✅ **Wake-on-LAN**: Native Kotlin implementation using UDP broadcast
- ✅ **Configuration UI**: Simple Android TV-optimized interface
- ✅ **Configurable Authentication**: Enable/disable API authentication via settings
- ✅ **Token Authentication**: Secure API access with Bearer tokens
- ✅ **Auto-start**: Automatically starts on Fire TV boot
- ✅ **IP Allowlist**: CIDR-based access control (optional)
- ✅ **Log Viewer**: Built-in log viewer for debugging
- ✅ **Persistent Config**: SharedPreferences-based configuration storage
- ✅ **Network Display**: Shows server IP address and port

## 📱 Screenshots

<img width="2377" height="1284" alt="image" src="https://github.com/user-attachments/assets/9eea48dc-0788-4f29-982d-cc434776da9a" />


Web interface
<img width="1938" height="1267" alt="image" src="https://github.com/user-attachments/assets/0c3dfacd-75fb-479b-9e2b-2381ad44e5ef" />



### Mobile Configuration Interface
- Password-protected settings with authentication toggle
- MAC address configuration with auto-formatting
- Token generation and visibility toggle  
- Service status with network address display
- Enable/disable authentication requirement

### Web Management Interface
- Login-protected access (default password: `admin123`)
- Complete configuration management with auth token integration
- One-click Wake-on-LAN with seamless authentication
- Real-time status display
- Professional dark theme with responsive design

## 🌐 API Endpoints

### Web Interface
- **GET /**: Password-protected web management interface
- **POST /login**: Web interface authentication (returns API token)

### API Endpoints  
- **GET /health**: Health check for monitoring (no auth required)
- **POST /wake**: Send Wake-on-LAN packet (requires auth if enabled)
- **GET /config**: Get current configuration (requires auth if enabled)
- **POST /config**: Update configuration (requires auth if enabled)

### Authentication System
- **Web Interface**: Password-based login (default: `admin123`) + automatic API token
- **API Access**: Configurable token-based authentication
  - **Enabled**: Requires Bearer token or query parameter `?token=xxx`
  - **Disabled**: Open access mode (no authentication required)
- **Default Token**: `default_token_change_me` (change immediately!)
- **Authentication Toggle**: Enable/disable via mobile app or web interface

## ⚙️ Configuration Parameters

- **Web Password**: Web interface access password (default: `admin123`)
- **Authentication Token**: API access token (default: `default_token_change_me`)
- **Require Authentication**: Enable/disable API authentication (default: enabled)
- **Target MAC Address**: PC MAC address for Wake-on-LAN (format: `XX:XX:XX:XX:XX:XX`)
- **Broadcast Address**: Network broadcast address (default: `255.255.255.255`)
- **WOL Port**: UDP port for magic packets (default: `9`)
- **HTTP Port**: HTTP server listening port (default: `8085`)
- **IP Allowlist**: CIDR notation IP restrictions (optional, comma-separated)
- **HTTPS Enabled**: Enable self-signed HTTPS (reserved for future use)
- **Auto-start**: Start service on Fire TV boot (default: enabled)

## 📦 Installation Instructions

### Prerequisites

1. **Fire TV Stick 4K**: Amazon Fire TV Stick 4K or compatible device
2. **ADB**: Android Debug Bridge for installation

### Quick Install (Pre-built APK)

1. **Download APK**: Get `WOLFireTV-v1.0.4-web-auth-fix.apk` from releases
2. **Enable Developer Options on Fire TV**:
   ```
   Settings → My Fire TV → About → Click "Build Version" 7 times
   Settings → My Fire TV → Developer Options → Enable "ADB Debugging" and "Apps from Unknown Sources"
   ```
3. **Install via ADB**:
   ```bash
   adb connect <FIRE_TV_IP>:5555
   adb install WOLFireTV-v1.0.4-web-auth-fix.apk
   ```

### Building from Source

1. **Clone Repository**:
   ```bash
   git clone https://github.com/vtstv/WOLFireTV.git
   cd WOLFireTV
   ```

2. **Build in Android Studio**:
   - Open project in Android Studio
   - Wait for Gradle sync
   - Build → Build Bundle(s) / APK(s) → Build APK(s)

3. **Command Line Build**:
   ```bash
   # Debug APK
   ./gradlew assembleDebug
   
   # Release APK
   ./gradlew assembleRelease
   ```

4. **Locate APK**:
   ```
   app/build/outputs/apk/debug/app-debug.apk
   app/build/outputs/apk/release/app-release.apk
   releases/WOLFireTV-ver.apk
   ```

### Initial Setup

1. **Launch Application**: Find "WOL Fire TV" in Fire TV Apps  
2. **Configure Settings**:
   - **Web Password**: Change from default `admin123`
   - **Authentication Token**: Change from default `default_token_change_me`
   - **Authentication Mode**: Enable/disable API authentication as needed
   - **Target MAC Address**: Enter PC's MAC address (auto-formatted as `XX:XX:XX:XX:XX:XX`)
   - **Network Settings**: Configure broadcast address and ports as needed
   - **Security**: Set IP allowlist if required (CIDR format)
3. **Start Service**: Enable and start the background service
4. **Verify Status**: Check service status shows correct network address

### Usage

#### Web Interface Access
```
http://<FIRE_TV_IP>:8085
```
1. Enter web password (default: `admin123`)
2. Access full configuration and wake controls
3. Authentication handled automatically after login
4. Configure authentication requirements via web interface

#### API Access Examples

**Health Check** (No Auth Required):
```bash
curl http://<FIRE_TV_IP>:8085/health
```

**Wake Computer** (Auth Required if Enabled):
```bash
# With authentication enabled
curl -X POST "http://<FIRE_TV_IP>:8085/wake" \
     -H "Authorization: Bearer default_token_change_me"

# OR with query parameter
curl -X POST "http://<FIRE_TV_IP>:8085/wake?token=default_token_change_me"

# With authentication disabled
curl -X POST "http://<FIRE_TV_IP>:8085/wake"
```

**Get Configuration**:
```bash
# With authentication enabled  
curl -H "Authorization: Bearer default_token_change_me" \
     http://<FIRE_TV_IP>:8085/config

# With authentication disabled
curl http://<FIRE_TV_IP>:8085/config
```

**Update Configuration**:
```bash
# With authentication enabled
curl -X POST "http://<FIRE_TV_IP>:8085/config" \
     -H "Authorization: Bearer default_token_change_me" \
     -H "Content-Type: application/json" \
     -d '{"requireAuthentication": false}'

# With authentication disabled  
curl -X POST "http://<FIRE_TV_IP>:8085/config" \
     -H "Content-Type: application/json" \
     -d '{"targetMacAddress": "18:31:BF:6E:D5:BB"}'
```

#### Home Automation Integration

**Home Assistant**:
```yaml
switch:
  - platform: command_line
    switches:
      pc_wake:
        command_on: 'curl -X POST "http://192.168.1.100:8085/wake" -H "Authorization: Bearer default_token_change_me"'
        friendly_name: "Wake PC"
        
  # Alternative without authentication (if disabled)
  - platform: command_line  
    switches:
      pc_wake_open:
        command_on: 'curl -X POST "http://192.168.1.100:8085/wake"'
        friendly_name: "Wake PC (Open Mode)"
```

**Node-RED**:
```javascript
// HTTP Request node (with auth)
URL: http://192.168.1.100:8085/wake
Method: POST
Headers: {"Authorization": "Bearer default_token_change_me"}

// HTTP Request node (without auth, if disabled)
URL: http://192.168.1.100:8085/wake  
Method: POST
```

## 🔧 Troubleshooting

### App Not Visible in Fire TV Launcher
- Ensure `leanback` feature is enabled in AndroidManifest.xml
- Check app appears in Settings → Applications → Manage Installed Applications
- Try launching from there first

### Service Won't Start
```bash
# Check logs
adb logcat -s WolService:* WolHttpServer:* WakeOnLan:*

# Check port availability
adb shell netstat -tulpn | grep 8085
```

### Web Interface Access Issues
1. **Service Status**: Verify service is running with correct IP address
2. **Network**: Ensure Fire TV and client are on same network  
3. **Password**: Check web password in mobile app settings
4. **Firewall**: Verify port 8085 is accessible
5. **Authentication**: Check if "Require Authentication" is enabled in settings

### API Authentication Issues
1. **401 Unauthorized**: 
   - Check if authentication is enabled in settings
   - Verify token is correct (`default_token_change_me` by default)
   - Use Bearer token in header or `?token=xxx` query parameter
2. **Config Save Fails**: Ensure web interface login was successful (provides auth token automatically)

### Wake-on-LAN Not Working
1. **PC Setup**: Enable Wake-on-LAN in BIOS/UEFI and network adapter
2. **Network**: Verify Fire TV and PC are on same network segment
3. **MAC Address**: Confirm MAC address format and correctness
4. **Testing**: Use mobile app "Test Wake" button first

## 🔒 Security Features

- **Configurable Authentication**: Enable/disable API authentication as needed
- **Web Password Protection**: Secure web interface access  
- **Token Authentication**: API access control with Bearer tokens
- **IP Allowlisting**: Network-based access restrictions (CIDR notation)
- **Broadcast Isolation**: UDP packets sent only to configured network
- **Session Management**: Web interface provides automatic API token after login
- **Security Hardening**: Fixed authentication bypass vulnerabilities (v1.0.2+)

## 🛡️ Security Fixes (v1.0.2 - v1.0.4)

### v1.0.2: Critical Authentication Fix
- **Fixed**: Authentication bypass when `authToken` was empty
- **Fixed**: Browser exemption allowing unauthorized access  
- **Added**: Proper conditional authentication based on `requireAuthentication` setting

### v1.0.3: Custom Icons  
- **Fixed**: App using XML vector icons instead of custom PNG icons
- **Removed**: Adaptive icon XML files that were overriding PNG icons

### v1.0.4: Web Interface Authentication
- **Fixed**: Web interface 401 errors when saving configuration
- **Added**: Automatic API token provision after web login  
- **Enhanced**: Seamless authentication flow between web login and API calls

## 🛠️ Development

### Dependencies
- **NanoHTTPD 2.3.1**: Lightweight HTTP server
- **Gson 2.10.1**: JSON serialization
- **Kotlinx Coroutines**: Async operations
- **AndroidX Libraries**: Modern Android components

### Fire TV Optimizations
- **Leanback Theme**: TV-optimized interface
- **D-pad Navigation**: Remote-friendly controls
- **Background Services**: Proper foreground service implementation
- **Resource Efficiency**: Minimal memory footprint

## 📝 Version History

### v1.0.4 (Current) - Web Authentication Fix
- **Fixed**: Web interface 401 errors when saving configuration
- **Added**: Automatic API token provision after web login
- **Enhanced**: Seamless authentication flow between web and API
- **Improved**: Error handling and user experience

### v1.0.3 - Custom Icons Fix  
- **Fixed**: App using XML vector icons instead of custom PNG icons
- **Removed**: Adaptive icon XML files overriding PNG icons
- **Improved**: Icon consistency across all Android versions

### v1.0.2 - Critical Security Fix
- **Fixed**: Authentication bypass vulnerability when token was empty
- **Fixed**: Browser exemption allowing unauthorized access
- **Added**: Proper conditional authentication logic
- **Secured**: All endpoints now respect authentication settings

### v1.0.1 - Authentication Configuration
- **Added**: Configurable authentication requirement setting
- **Added**: Enable/disable authentication via mobile app and web interface  
- **Added**: UI controls for authentication toggle
- **Enhanced**: Security model with optional authentication

### v1.0.0 - Initial Release
- Complete web interface with password protection
- Token-based API authentication
- Wake-on-LAN functionality with UDP broadcast
- Fire TV optimized interface with leanback theme
- Background service with persistent notification
- Comprehensive configuration management

## 📄 License

This project is open source and available under the MIT License.

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 🆘 Support

- **Issues**: Report bugs and feature requests on GitHub
- **Documentation**: Check this README and code comments
- **Logs**: Use built-in log viewer or `adb logcat`
- **Community**: Discussions welcome in GitHub Issues

## 👨‍💻 Author

**Murr**
- GitHub: [@vtstv](https://github.com/vtstv)
- Project: [WOL Fire TV](https://github.com/vtstv/WOLFireTV)

---

*Made with ❤️ for the Fire TV community*