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
