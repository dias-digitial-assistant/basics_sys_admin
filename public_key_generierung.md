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
