# Land of Light Gate

I’m sharing the address of my gate, Land of Light, for anyone who would like to use it to dial their own gate and check if it works:

https://gate.highlandergate.com/fan113.html

To test your gate, select the symbols of your own gate address and then press the dark red dot.

The same dot can also be used to disconnect the wormhole.

## Public gate use

The Land of Light public gate page is shared only for normal manual StargateProject fan testing.

Do not modify, impersonate, automate, scan, stress test, probe, bypass protection, or attempt to access private controls or network resources.

See [LICENSE](LICENSE) for the full public gate access terms.

## How to access your own Stargate web interface from outside your home network

This section is general advice for Stargate builders who want to make their own SG1 graphical web interface reachable from outside their home network.

This is not a required SG1 add-on and it is not a replacement for the original StargateProject software. It is a guide for safely exposing your own gate web interface by using Cloudflare Tunnel instead of opening router ports.

The SG1 software already includes a graphical web interface. On your local network it is usually available at:

```text
http://stargate.local/
```

or:

```text
http://YOUR_PI_IP_ADDRESS/
```

If you want to control the gate from outside your home network, the safest way is to use Cloudflare Tunnel instead of opening ports on your router.

### 1. Install SG1 on the Raspberry Pi

First make sure the SG1 software is installed and working locally.

Open the web interface from another device on the same network:

```text
http://stargate.local/
```

or:

```text
http://YOUR_PI_IP_ADDRESS/
```

Do not continue until the local web interface works.

### 2. Use a domain connected to Cloudflare

You need a domain name managed by Cloudflare, for example:

```text
yourdomain.com
```

### 3. Install Cloudflare Tunnel on the Raspberry Pi

Install `cloudflared` on the Raspberry Pi.

Official Cloudflare Tunnel setup guide:

```text
https://developers.cloudflare.com/tunnel/setup/
```

### 4. Create a Cloudflare Tunnel

In Cloudflare go to:

```text
Zero Trust -> Networks -> Tunnels -> Create Tunnel
```

Choose Cloudflared, then follow the Linux, Debian, or Raspberry Pi instructions shown by Cloudflare.

Cloudflare will give you a command to run on the Raspberry Pi.

### 5. Add a public hostname

Inside the tunnel settings, add a public hostname such as:

```text
gate.yourdomain.com
```

Point it to the local SG1 web server:

```text
http://localhost:80
```

Usually SG1 is served through Apache on port 80, so `http://localhost:80` is the correct target.

### 6. Run the tunnel as a service

Install the tunnel as a system service so it starts automatically after reboot.

Official guide:

```text
https://developers.cloudflare.com/tunnel/advanced/local-management/as-a-service/linux/
```

Typical commands are:

```bash
sudo cloudflared service install
sudo systemctl start cloudflared
sudo systemctl status cloudflared
```

### 7. Protect the web interface

Do not expose full SG1 control publicly without protection.

In Cloudflare Zero Trust, create an Access Application for:

```text
gate.yourdomain.com
```

Then require login by email, PIN, one-time code, or another Cloudflare Access policy.

### 8. Test from outside your home network

Turn off Wi-Fi on your phone and use mobile data.

Open:

```text
https://gate.yourdomain.com/
```

If everything is configured correctly, the SG1 web interface should load from outside your home network.

### 9. Optional: create a custom control page

The normal SG1 interface is already graphical.

If you want a custom dashboard with a camera view, a gate window, or simplified controls, you can create a custom HTML page inside:

```text
/home/pi/sg1_v4/web/
```

For example:

```text
/home/pi/sg1_v4/web/control.html
```

Then it can be opened as:

```text
https://gate.yourdomain.com/control.html
```

### 10. Optional: create a guest or fan page

If other people should be allowed to dial your gate, do not give them full admin access.

Create a limited page that only allows dialing, for example:

```text
https://gate.yourdomain.com/fan.html
```

This page should not expose configuration, debug controls, logs, or system settings.

### Short summary

Install SG1 and confirm the local web interface works.

Use Cloudflare Tunnel to connect:

```text
gate.yourdomain.com -> http://localhost:80
```

Protect it with Cloudflare Access.

Then you can safely reach the SG1 graphical web interface from outside your home network without opening router ports.
