# x2gousbmount_gphoto
Modification of x2go thinclient scripts to mount mmc cards and gphotofs (mtp cameras - canon etc.) and android smartphones in camera (PTP) mode.
Do not forget to backup everything you replace or change!

0. install `gphotofs`
1. Modify `62-x2gogphotomount.rules` to add your camera/phone using usb vendor id and model (look in lsusb)
3. put udev rules in `/lib/udev/rules.d/` (replace is necessary as in other case you will get double mounts of mass storages)
4. put `gphotofs@.service` in `/etc/systemd/system`
5. replace `/usr/lib/x2go/tce/x2gousbmount` with file from repo
6. reboot or restart udev by `udevadm control --reload-rules && udevadm trigger`
7. If you have problems with character set of filenames in mounted folders - replace `/usr/bin/x2gomountdirs` script on server side (by default x2go tries to change encoding when mountig sshfs, even if it is not necessary).

TOFIX: encrypted cards forwarding won't work as systemctl start will always retrun 0.

# x2gomatebindings locale (language) issue fix
Bug is submitted here: http://bugs.x2go.org/cgi-bin/bugreport.cgi?bug=1424. 
If you have problems with wrong locale in Mate (ex. english folder names instead of names in language set by locale) do on the server side:
1. Create symlink for locales folder `cd /usr/local/share ; ln -s /usr/share/locale`
2. Take correct file with full data for your locale from here https://github.com/mate-desktop/caja/tree/master/po 
3. (optional) Add strings in your language for SSHFS unmounting at the end of the downloaded locale file:
```
#: ../extension/caja-x2goumount.c:77 ../extension/caja-x2goumount.c:78
msgid "Unmount SSHFS Drive"
msgstr "Отмонтировать SSHFS устройство"
```
4. Generate locale .mo file `msgfmt <yourlocale>.po -o x2goumount.mo` and put it on the server in `/usr/local/share/locale/<your_locale>/`

*Alternatively you can build patched x2gomatebindings. Use `x2gomatebindings_locale.patch`
You only need to replace `/lib/x86_64-linux-gnu/caja/extensions-2.0/libcaja-x2goumount.so` with built lib. But all the changes wil be overwritten if the developers of x2go release new version, so for now suggested workaround is better than patching. 
UPD: bug opened here https://bugs.x2go.org/cgi-bin/bugreport.cgi?bug=1424 patch submitted, hopefully will be fixed in 0.0.1.7 package version
        


