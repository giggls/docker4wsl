# Docker im Windows Subsystem for Linux laufen lassen

## Installation im Windows Subsystem for Linux 2 (WSL 2)

Seit Windows 10 in der Version 2004 Build 19041 oder höher kann Docker
unverändert unter Windows unter Verwendung der Linuxdistribution seiner Wahl
laufen.


Hierzu installiert man zunächst **WSL2** über folgende Schritte
(siehe auch https://docs.microsoft.com/en-us/windows/wsl/install-win10):

1. Befehle mit Administratorrechten ausführen:

   ```
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```
2. Reboot

3. Befehl mit Administratorrechten ausführen:
   ```
   wsl --set-default-version 2

   ```
4. Ggfs. Update der Kernelkomponente (https://aka.ms/wsl2kernel)

   Download und ausführen von https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
   Schritt 3 erneut ausführen.

5. Alle weiteren Befehle erfordern keine Administratorrechte

6. Linuxdistribution (Debian 10) aus dem Microsoft Store installieren

   Eine Anmeldung im Microsoft-Store ist dazu nicht notwendig, auch wenn das
   UI einen anderen Eindruck erweckt.

7. Sichertellen das Debian unter WSL 2 läuft und ggfs. umstellen.

   ```
   H:\>wsl -l -v
     NAME      STATE           VERSION
   * Debian    Stopped         1      
   H:\>wsl --set-version Debian 2
   Die Konvertierung wird ausgeführt. Dieser Vorgang kann einige Minuten dauern...
   Informationen zu den wichtigsten Unterschieden zu WSL 2 finden Sie unter https://aka.ms/wsl2
   Die Konvertierung ist beendet.

   H:\>
   ```

## Installation von Docker und Co. in Debian/WSL 2

1. Distribution auf aktuellen Stand bringen

   ```
   sudo -s
   apt update; apt dist-upgrade
   ```
2. Das Programm ``iptables`` auf legacy umstellen

   ```
   update-alternatives --set iptables /usr/sbin/iptables-legacy
   update-alternatives: using /usr/sbin/iptables-legacy to provide /usr/sbin/iptables (iptables) in manual mode
   ```

3. Docker installieren
   ```
   apt install docker.io
   ```
4. Dockerd im Hintergrund starten

   ```
   start-stop-daemon --start -b --exec /usr/sbin/dockerd
   ```

5. Docker image hello-world ausführen
   
   ```
   root@wsl-machine:~# docker run hello-world
   Unable to find image 'hello-world:latest' locally
   latest: Pulling from library/hello-world
   0e03bdcc26d7: Pull complete
   Digest: sha256:49a1c8800c94df04e9658809b006fd8a686cab8028d33cfba2cc049724254202
   Status: Downloaded newer image for hello-world:latest

   Hello from Docker!
   This message shows that your installation appears to be working correctly.

   To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
       (amd64)
    3. The Docker daemon created a new container from that image which runs the
       executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
       to your terminal.

   To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash

   Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/

   For more examples and ideas, visit:
    https://docs.docker.com/get-started/
   ```


