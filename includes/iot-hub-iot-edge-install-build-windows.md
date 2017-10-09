## <a name="install-hello-prerequisites"></a>Instalace požadovaných součástí hello

1. Nainstalujte [Visual Studio 2015 nebo 2017](https://www.visualstudio.com). Můžete použít hello volné Community Edition, pokud splňují hello licenční požadavky. Být, že tooinclude Visual C++ a Správce balíčků NuGet.

1. Nainstalujte [git](http://www.git-scm.com) a zajistěte, aby git.exe můžete spustit z příkazového řádku hello.

1. Nainstalujte [CMake](https://cmake.org/download/) a zajistěte, aby cmake.exe můžete spustit z příkazového řádku hello. CMake verze 3.7.2 nebo novější je doporučené. Hello **.msi** instalační program je nejjednodušší možnost hello v systému Windows. Přidat CMake toohello CESTU pro alespoň hello aktuálního uživatele, když hello instalační program zobrazí výzvu.

1. Nainstalujte [Python 2.7](https://www.python.org/downloads/release/python-27). Zajistěte, aby přidáte Python tooyour `PATH` proměnné prostředí v **ovládací panely -> Systém -> Upřesnit nastavení systému -> proměnné prostředí**.

1. Na příkazovém řádku spusťte následující příkaz tooclone hello Azure IoT Edge Githubu úložiště tooyour místního počítače hello:

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-toobuild-hello-sample"></a>Jak toobuild hello ukázka

Teď můžete sestavit hello IoT Edge runtime a ukázky na místním počítači:

1. Otevřete **příkazový řádek vývojáře pro VS 2015** nebo **příkazový řádek vývojáře pro VS 2017** příkazového řádku.

1. Přejděte toohello kořenovou složku v místní kopii hello **iot hranou** úložiště.

1. Spusťte skript sestavení následujícím způsobem:

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

Tento skript vytvoří soubor řešení sady Visual Studio a vytvoří řešení hello. Hello řešení sady Visual Studio můžete najít v hello **sestavení** složky ve vaší místní kopii hello **iot hranou** úložiště. Pokud chcete toobuild a spouštění testování částí hello, přidejte hello `--run-unittests` parametr. Pokud chcete toobuild a testy tooend hello end, přidejte hello `--run-e2e-tests`.

> [!NOTE]
> Pokaždé, když spustíte hello **build.cmd** skriptu, se odstraní a potom znovu vytvoří hello **sestavení** složky v kořenové složce hello místní kopii hello **iot hranou** úložiště.
