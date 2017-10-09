## <a name="install-hello-prerequisites"></a>Instalace požadovaných součástí hello

Hello postup v tomto kurzu se předpokládá, že používáte Ubuntu Linux.

Otevřete prostředí a spusťte následující příkazy tooinstall hello požadované balíčky hello:

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

V prostředí hello spusťte následující příkaz tooclone hello Azure IoT Edge Githubu úložiště tooyour místního počítače hello:

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-toobuild-hello-sample"></a>Jak toobuild hello ukázka

Teď můžete sestavit hello IoT Edge runtime a ukázky na místním počítači:

1. Otevřete prostředí.

1. Přejděte toohello kořenovou složku v místní kopii hello **iot hranou** úložiště.

1. Spusťte skript sestavení následujícím způsobem:

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

Tento skript používá **cmake** toocreate nástroj složky s názvem **sestavení** hello kořenové složky vaší místní kopii **iot hranou** úložiště a generování souboru pravidel. Hello skript potom vytvoří hello řešení, přeskočení testy částí a end tooend testy. Pokud chcete toobuild a spouštění testování částí hello, přidejte hello `--run-unittests` parametr. Pokud chcete toobuild a testy tooend hello end, přidejte hello `--run-e2e-tests`.

> [!NOTE]
> Pokaždé, když spustíte hello **build.sh** skriptu, se odstraní a potom znovu vytvoří hello **sestavení** složky v kořenové složce hello místní kopii hello **iot hranou** úložiště.
