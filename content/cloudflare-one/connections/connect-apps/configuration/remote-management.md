---
pcx-content-type: how-to
title: Remote management
weight: 2
---

# Remote management

If you created a Cloudflare Tunnel from the Zero Trust Dashboard, the tunnel [runs as a service](/cloudflare-one/connections/connect-apps/run-tunnel/as-a-service/) on your OS. You can modify the Cloudflare Tunnel service with one or more [configuration options](/cloudflare-one/connections/connect-apps/configuration/arguments/).

## Linux

On Linux, Cloudflare Tunnel installs itself as a system service using `systemctl`. By default, the service will be named `cloudfared.service`. To configure your tunnel on Linux:

1. Open `cloudflared.service`.

    ```bash
    sudo systemctl edit --full cloudflared.service
    ```

2. Modify the `cloudflared tunnel run` command with the desired configuration flag. The following example changes the tunnel `protocol` to QUIC:

    ```txt
    ---
    highlight: [8]
    ---
    [Unit]
    Description=Cloudflare Tunnel
    After=network.target
   
    [Service]
    TimeoutStartSec=0
    Type=notify
    ExecStart=/usr/local/bin/cloudflared --protocol quic tunnel run --token <TOKEN VALUE>
    Restart=on-failure
    RestartSec=5s
    ```

## MacOS

On MacOS, Cloudflare Tunnel installs itself as a launch agent using `launchctl`. By default, the agent will be called `com.cloudflare.cloudflared`. To configure your tunnel on MacOS:

1. Stop the `cloudflared` service.

    ```bash
    sudo launchctl stop com.cloudflare.cloudflared
    ```

2. Unload the configuration file.

    ```bash
    sudo launchctl unload /Library/LaunchDaemons/com.cloudflare.cloudflared.plist
    ```

3. Open `/Library/LaunchDaemons/com.cloudflare.cloudflared.plist` in a text editor.

4. Modify the `ProgramArguments` key with the desired configuration flag. The following example changes the tunnel `protocol` to QUIC:

    ```txt
    ---
    highlight: [8,9]
    ---
    <plist version="1.0">
        <dict>
            <key>Label</key>
            <string>com.cloudflare.cloudflared</string>
            <key>ProgramArguments</key>
            <array>
                <string>/opt/homebrew/bin/cloudflared</string>
                <string>--protocol</string>
                <string>quic</string>
                <string>tunnel</string>
                <string>run</string>
                <string>--token</string>
                <string>TOKEN VALUE </string>
            </array>
    ```

5. Load the updated configuration file.

    ```bash
    sudo launchctl load /Library/LaunchDaemons/com.cloudflare.cloudflared.plist
    ```

6. Start the `cloudflared` service.

    ```bash
    sudo launchctl start com.cloudflare.cloudflared
    ```

## Windows

On Windows, Cloudflare Tunnel installs itself as a system service using the Registry Editor. By default, the service will be named `cloudfared`. To configure your tunnel on Windows:

1. Open the Registry Editor.

2. Navigate to **HKEY_LOCAL_MACHINE** > **SYSTEM** > **CurrentControlSet** > **Services** > **cloudflared**.

3. Double-click **ImagePath**.

4. Modify **Value data** with the desired configuration flag. The following example changes the tunnel `protocol` to QUIC:

    ```txt
    C:\Program Files (x86)\cloudflared\.\cloudflared.exe --protocol quic tunnel run --token <TOKEN VALUE>
    ```

![Modify cloudflared service in the Registry Editor](/cloudflare-one/static/documentation/connections/connect-apps/remote-management-windows.png)
