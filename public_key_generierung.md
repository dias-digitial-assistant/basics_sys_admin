# Wie man ein SSH-Schlüsselpaar generiert

Folgen Sie diesen Schritten, um Ihr SSH-Schlüsselpaar zu generieren:

1. Öffnen Sie ein Terminal (Eingabeaufforderung unter Windows, Terminal unter macOS/Linux).

2. Führen Sie den folgenden Befehl aus:
   ```
   ssh-keygen -t ed25519 -C "ihre_email@beispiel.com"
   ```
   Ersetzen Sie "ihre_email@beispiel.com" durch Ihre tatsächliche E-Mail-Adresse.

3. Wenn Sie aufgefordert werden "Geben Sie die Datei ein, in der der Schlüssel gespeichert werden soll", drücken Sie Enter, um den Standardspeicherort zu akzeptieren.

4. Sie werden aufgefordert, eine Passphrase einzugeben. Für zusätzliche Sicherheit geben Sie eine starke Passphrase ein. Sie können auch Enter drücken, um keine Passphrase zu verwenden, aber dies ist weniger sicher.

5. Ihre öffentlichen und privaten Schlüssel werden generiert. Der öffentliche Schlüssel wird mit der Erweiterung .pub gespeichert.

6. Um Ihren öffentlichen Schlüssel anzuzeigen, verwenden Sie einen dieser Befehle:
   - Unter macOS/Linux: `cat ~/.ssh/id_ed25519.pub`
   - Unter Windows: `type %UserProfile%\.ssh\id_ed25519.pub`

7. Kopieren Sie die gesamte Ausgabe dieses Befehls. Dies ist Ihr öffentlicher Schlüssel, der so aussehen sollte:
   ```
   ssh-ed25519 AAAA... ihre_email@beispiel.com
   ```

8. Senden Sie nur diesen öffentlichen Schlüssel an Ihren Systemadministrator. Teilen Sie niemals Ihren privaten Schlüssel.

Denken Sie daran: Bewahren Sie Ihren privaten Schlüssel sicher auf und teilen Sie ihn niemals mit jemand anderem!

---

# Sich mit Ihrem SSH-Schlüssel am Server anmelden

Sobald Ihr Administrator bestätigt hat, dass Ihr öffentlicher Schlüssel auf dem Server hinterlegt wurde, können Sie sich anmelden. Sie erhalten vom Administrator folgende Informationen:

- **Hostname** (IP-Adresse oder Domainname des Servers)
- **Benutzername** (Ihr Konto auf dem Server)
- **Port** (Standard ist 22, kann aber abweichen)

## Linux

1. Öffnen Sie ein Terminal.

2. Verbinden Sie sich mit dem Server:
   ```bash
   ssh benutzername@hostname -p portnummer
   ```
   Beispiel:
   ```bash
   ssh meinbenutzer@192.xxx.xxx.xxx -p 22
   ```
   Wenn der Port 22 ist (Standard), können Sie `-p 22` weglassen.

3. Beim ersten Verbinden werden Sie gefragt, ob Sie dem Server vertrauen möchten:
   ```
   The authenticity of host '192.xxx.xxx.xxx (192.xxx.xxx.xxx)' can't be established.
   ED25519 key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxx.
   Are you sure you want to continue connecting (yes/no/[fingerprint])?
   ```
   Geben Sie `yes` ein und drücken Sie Enter. Diese Meldung erscheint nur beim ersten Mal.

4. Wenn Sie eine Passphrase für Ihren Schlüssel gesetzt haben, werden Sie jetzt danach gefragt. Geben Sie sie ein.

5. Sie sind nun angemeldet.

### SSH-Konfigurationsdatei einrichten (empfohlen)

Um sich das Eintippen der Verbindungsdetails zu ersparen, können Sie eine Konfigurationsdatei anlegen:

1. Erstellen bzw. bearbeiten Sie die Datei `~/.ssh/config`:
   ```bash
   nano ~/.ssh/config
   ```

2. Fügen Sie einen Eintrag für Ihren Server hinzu:
   ```
   Host meinserver
     HostName 192.xxx.xxx.xxx
     User meinbenutzer
     Port 22
     IdentityFile ~/.ssh/id_ed25519
   ```

3. Speichern Sie die Datei (in nano: `Strg+O`, dann `Strg+X`).

4. Setzen Sie die richtigen Berechtigungen:
   ```bash
   chmod 600 ~/.ssh/config
   ```

5. Ab jetzt können Sie sich einfach so verbinden:
   ```bash
   ssh meinserver
   ```

## macOS

Die Vorgehensweise ist identisch mit Linux (siehe oben). macOS hat ein eingebautes Terminal und einen SSH-Client.

1. Öffnen Sie die App **Terminal** (zu finden unter Programme > Dienstprogramme > Terminal, oder über Spotlight-Suche mit `Cmd+Leertaste` und Eingabe von "Terminal").

2. Folgen Sie den gleichen Schritten wie unter Linux beschrieben.

### macOS-Besonderheit: Passphrase im Schlüsselbund speichern

Damit Sie die Passphrase nicht bei jeder Verbindung erneut eingeben müssen:

1. Bearbeiten Sie `~/.ssh/config` und fügen Sie hinzu:
   ```
   Host *
     AddKeysToAgent yes
     UseKeychain yes
   ```

2. Beim nächsten Verbinden wird Ihre Passphrase im macOS-Schlüsselbund gespeichert.

## Windows

### Option A: Windows Terminal / PowerShell (Windows 10/11)

Windows 10 und 11 haben einen eingebauten SSH-Client.

1. Öffnen Sie **Windows Terminal** oder **PowerShell** (Rechtsklick auf Start-Button > "Terminal" oder "Windows PowerShell").

2. Verbinden Sie sich mit dem Server:
   ```powershell
   ssh benutzername@hostname -p portnummer
   ```
   Beispiel:
   ```powershell
   ssh meinbenutzer@192.xxx.xxx.xxx -p 22
   ```

3. Beim ersten Verbinden bestätigen Sie den Fingerabdruck des Servers mit `yes`.

4. Geben Sie Ihre Passphrase ein, falls Sie eine gesetzt haben.

5. Sie sind nun angemeldet.

#### SSH-Konfigurationsdatei unter Windows

1. Erstellen bzw. bearbeiten Sie die Datei `C:\Users\IhrName\.ssh\config` mit einem Texteditor (z.B. Notepad):
   ```powershell
   notepad %UserProfile%\.ssh\config
   ```

2. Fügen Sie Ihren Servereintrag hinzu:
   ```
   Host meinserver
     HostName 192.xxx.xxx.xxx
     User meinbenutzer
     Port 22
     IdentityFile C:\Users\IhrName\.ssh\id_ed25519
   ```

3. Speichern und schließen. Ab jetzt:
   ```powershell
   ssh meinserver
   ```

### Option B: PuTTY (ältere Windows-Versionen)

Wenn Sie eine ältere Windows-Version nutzen oder PuTTY bevorzugen:

1. Laden Sie [PuTTY](https://www.putty.org/) herunter und installieren Sie es.

2. Konvertieren Sie Ihren Schlüssel mit **PuTTYgen**:
   - Öffnen Sie PuTTYgen (wird mit PuTTY installiert).
   - Klicken Sie auf "Load" und wählen Sie Ihre Datei `C:\Users\IhrName\.ssh\id_ed25519` (Dateityp auf "All Files" stellen).
   - Klicken Sie auf "Save private key", um eine `.ppk`-Datei zu speichern.

3. Verbinden Sie sich mit **PuTTY**:
   - Öffnen Sie PuTTY.
   - Geben Sie unter "Host Name" die IP-Adresse ein (z.B. `192.xxx.xxx.xxx`).
   - Geben Sie unter "Port" den Port ein (z.B. `22`).
   - Navigieren Sie links zu **Connection > SSH > Auth > Credentials**.
   - Wählen Sie bei "Private key file for authentication" Ihre `.ppk`-Datei aus.
   - Gehen Sie zurück zu **Session**, geben Sie einen Namen unter "Saved Sessions" ein und klicken Sie "Save".
   - Klicken Sie "Open", um die Verbindung herzustellen.

## Visual Studio Code (alle Betriebssysteme)

VS Code bietet eine komfortable Möglichkeit, sich per SSH mit einem Server zu verbinden und direkt auf dem Server zu arbeiten.

### Einrichtung

1. Installieren Sie die Erweiterung **Remote - SSH**:
   - Öffnen Sie VS Code.
   - Drücken Sie `Strg+Shift+X` (Windows/Linux) oder `Cmd+Shift+X` (macOS), um die Erweiterungsansicht zu öffnen.
   - Suchen Sie nach "Remote - SSH" (Herausgeber: Microsoft).
   - Klicken Sie auf "Installieren".

2. Richten Sie die SSH-Konfiguration ein:
   - Drücken Sie `Strg+Shift+P` (Windows/Linux) oder `Cmd+Shift+P` (macOS), um die Befehlspalette zu öffnen.
   - Geben Sie ein: `Remote-SSH: Open SSH Configuration File`
   - Wählen Sie Ihre Konfigurationsdatei:
     - Linux/macOS: `~/.ssh/config`
     - Windows: `C:\Users\IhrName\.ssh\config`
   - Fügen Sie Ihren Servereintrag hinzu (falls noch nicht vorhanden):
     ```
     Host meinserver
       HostName 192.xxx.xxx.xxx
       User meinbenutzer
       Port 22
       IdentityFile ~/.ssh/id_ed25519
     ```
     Unter Windows verwenden Sie stattdessen: `IdentityFile C:\Users\IhrName\.ssh\id_ed25519`
   - Speichern Sie die Datei.

### Verbinden

1. Drücken Sie `Strg+Shift+P` / `Cmd+Shift+P` und geben Sie ein: `Remote-SSH: Connect to Host`
2. Wählen Sie Ihren Server aus der Liste (z.B. "meinserver").
3. Ein neues VS Code-Fenster öffnet sich und verbindet sich mit dem Server.
4. Wenn Sie eine Passphrase gesetzt haben, werden Sie danach gefragt.
5. Wählen Sie beim ersten Verbinden das Betriebssystem des Servers (in der Regel "Linux").
6. Sie können nun Ordner auf dem Server öffnen, Dateien bearbeiten, und das integrierte Terminal nutzen — alles direkt in VS Code.

### Tipps für VS Code

- Das integrierte Terminal (`Strg+ö` oder `` Strg+` ``) läuft direkt auf dem Server.
- Sie können Dateien per Drag-and-Drop zwischen Ihrem lokalen Rechner und dem Server übertragen.
- Erweiterungen können direkt auf dem Server installiert werden, um z.B. Syntax-Highlighting oder Linting für Serverdateien zu nutzen.

---

# How to Generate Your SSH Key Pair

Follow these steps to generate your SSH key pair:

1. Open a terminal (Command Prompt on Windows, Terminal on macOS/Linux).

2. Run the following command:
   ```
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
   Replace "your_email@example.com" with your actual email address.

3. When prompted "Enter file in which to save the key," press Enter to accept the default location.

4. You'll be asked to enter a passphrase. For added security, enter a strong passphrase. You can also press Enter for no passphrase, but this is less secure.

5. Your public and private keys will be generated. The public key will be saved with a .pub extension.

6. To view your public key, use one of these commands:
   - On macOS/Linux: `cat ~/.ssh/id_ed25519.pub`
   - On Windows: `type %UserProfile%\.ssh\id_ed25519.pub`

7. Copy the entire output of this command. This is your public key, which should look like:
   ```
   ssh-ed25519 AAAA... your_email@example.com
   ```

8. Send only this public key to your system administrator. Never share your private key.

Remember: Keep your private key secure and never share it with anyone!

---

# Logging In to the Server with Your SSH Key

Once your administrator has confirmed that your public key has been added to the server, you can log in. Your administrator will provide you with:

- **Hostname** (IP address or domain name of the server)
- **Username** (your account on the server)
- **Port** (default is 22, but it may differ)

## Linux

1. Open a terminal.

2. Connect to the server:
   ```bash
   ssh username@hostname -p portnumber
   ```
   Example:
   ```bash
   ssh myuser@192.xxx.xxx.xxx -p 22
   ```
   If the port is 22 (default), you can omit `-p 22`.

3. On first connection, you will be asked to trust the server:
   ```
   The authenticity of host '192.xxx.xxx.xxx (192.xxx.xxx.xxx)' can't be established.
   ED25519 key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxx.
   Are you sure you want to continue connecting (yes/no/[fingerprint])?
   ```
   Type `yes` and press Enter. This message only appears on the first connection.

4. If you set a passphrase for your key, you will be prompted for it now. Enter it.

5. You are now logged in.

### Setting Up an SSH Config File (Recommended)

To avoid typing the connection details every time, you can create a configuration file:

1. Create or edit the file `~/.ssh/config`:
   ```bash
   nano ~/.ssh/config
   ```

2. Add an entry for your server:
   ```
   Host myserver
     HostName 192.xxx.xxx.xxx
     User myuser
     Port 22
     IdentityFile ~/.ssh/id_ed25519
   ```

3. Save the file (in nano: `Ctrl+O`, then `Ctrl+X`).

4. Set the correct permissions:
   ```bash
   chmod 600 ~/.ssh/config
   ```

5. From now on, you can simply connect with:
   ```bash
   ssh myserver
   ```

## macOS

The process is identical to Linux (see above). macOS has a built-in terminal and SSH client.

1. Open the **Terminal** app (found under Applications > Utilities > Terminal, or use Spotlight search with `Cmd+Space` and type "Terminal").

2. Follow the same steps as described under Linux.

### macOS-Specific: Save Passphrase in Keychain

To avoid entering your passphrase on every connection:

1. Edit `~/.ssh/config` and add:
   ```
   Host *
     AddKeysToAgent yes
     UseKeychain yes
   ```

2. On the next connection, your passphrase will be saved in the macOS Keychain.

## Windows

### Option A: Windows Terminal / PowerShell (Windows 10/11)

Windows 10 and 11 have a built-in SSH client.

1. Open **Windows Terminal** or **PowerShell** (right-click the Start button > "Terminal" or "Windows PowerShell").

2. Connect to the server:
   ```powershell
   ssh username@hostname -p portnumber
   ```
   Example:
   ```powershell
   ssh myuser@192.xxx.xxx.xxx -p 22
   ```

3. On first connection, confirm the server fingerprint by typing `yes`.

4. Enter your passphrase if you set one.

5. You are now logged in.

#### SSH Config File on Windows

1. Create or edit the file `C:\Users\YourName\.ssh\config` with a text editor (e.g., Notepad):
   ```powershell
   notepad %UserProfile%\.ssh\config
   ```

2. Add your server entry:
   ```
   Host myserver
     HostName 192.xxx.xxx.xxx
     User myuser
     Port 22
     IdentityFile C:\Users\YourName\.ssh\id_ed25519
   ```

3. Save and close. From now on:
   ```powershell
   ssh myserver
   ```

### Option B: PuTTY (Older Windows Versions)

If you are using an older Windows version or prefer PuTTY:

1. Download and install [PuTTY](https://www.putty.org/).

2. Convert your key with **PuTTYgen**:
   - Open PuTTYgen (installed with PuTTY).
   - Click "Load" and select your file `C:\Users\YourName\.ssh\id_ed25519` (set file type to "All Files").
   - Click "Save private key" to save a `.ppk` file.

3. Connect with **PuTTY**:
   - Open PuTTY.
   - Enter the IP address under "Host Name" (e.g., `192.xxx.xxx.xxx`).
   - Enter the port under "Port" (e.g., `22`).
   - Navigate on the left to **Connection > SSH > Auth > Credentials**.
   - Select your `.ppk` file under "Private key file for authentication".
   - Go back to **Session**, enter a name under "Saved Sessions" and click "Save".
   - Click "Open" to establish the connection.

## Visual Studio Code (All Operating Systems)

VS Code provides a convenient way to connect to a server via SSH and work directly on the server.

### Setup

1. Install the **Remote - SSH** extension:
   - Open VS Code.
   - Press `Ctrl+Shift+X` (Windows/Linux) or `Cmd+Shift+X` (macOS) to open the Extensions view.
   - Search for "Remote - SSH" (publisher: Microsoft).
   - Click "Install".

2. Set up the SSH configuration:
   - Press `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (macOS) to open the Command Palette.
   - Type: `Remote-SSH: Open SSH Configuration File`
   - Select your configuration file:
     - Linux/macOS: `~/.ssh/config`
     - Windows: `C:\Users\YourName\.ssh\config`
   - Add your server entry (if not already present):
     ```
     Host myserver
       HostName 192.xxx.xxx.xxx
       User myuser
       Port 22
     ```
   - Save the file.

### Connecting

1. Press `Ctrl+Shift+P` / `Cmd+Shift+P` and type: `Remote-SSH: Connect to Host`
2. Select your server from the list (e.g., "myserver").
3. A new VS Code window will open and connect to the server.
4. If you set a passphrase, you will be prompted for it.
5. On first connection, select the operating system of the server (usually "Linux").
6. You can now open folders on the server, edit files, and use the integrated terminal — all directly in VS Code.

### Tips for VS Code

- The integrated terminal (`` Ctrl+` ``) runs directly on the server.
- You can transfer files between your local machine and the server via drag-and-drop.
- Extensions can be installed directly on the server for syntax highlighting, linting, and more.
