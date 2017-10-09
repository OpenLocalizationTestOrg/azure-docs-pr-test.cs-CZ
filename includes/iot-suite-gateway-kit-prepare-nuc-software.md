## <a name="build-iot-edge"></a>Sestavení IoT Edge

Tento kurz používá vlastní moduly toocommunicate IoT Edge s hello předkonfigurovanému řešení vzdáleného monitorování. Proto musíte toobuild hello IoT Edge moduly z vlastní zdrojového kódu. Hello následující části popisují, jak tooinstall IoT okraj a sestavení hello vlastní modul IoT okraj.

### <a name="install-iot-edge"></a>Nainstalujte IoT Edge

Hello následující kroky popisují, jak tooinstall hello předem zkompilovat IoT Edge software na hello Intel NUC:

1. Konfigurace úložiště inteligentní balíček hello požadované spuštěním následujících příkazů na hello Intel NUC hello:

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    Zadejte `y` při hello příkaz vás vyzve příliš**zahrnout tento kanál?**.

1. Aktualizujte správce balíčků inteligentní hello spuštěním hello následující příkaz:

    ```bash
    smart update
    ```

1. Instalovat balíček Azure IoT Edge hello spuštěním hello následující příkaz:

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. Ověření instalace hello ve spuštěné ukázka hello "Hello, world". Tato ukázka zapíše soubor hello world zprávy toohello log.txT každých pět sekund. Hello následující příkazy hello "Hello, world" ukázku spustit:

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    Ignorovat jakékoli **neplatný argument** zprávy při zastavení ukázka hello.

    Použijte následující příkaz tooview hello obsahu souboru protokolu hello hello:

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a>Řešení potíží

Pokud se zobrazí chyba hello "žádný balíček poskytuje util. linux dev", zkuste restartovat hello Intel NUC.
