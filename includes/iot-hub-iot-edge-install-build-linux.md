## <a name="install-hello-prerequisites"></a><span data-ttu-id="7a256-101">Instalace požadovaných součástí hello</span><span class="sxs-lookup"><span data-stu-id="7a256-101">Install hello prerequisites</span></span>

<span data-ttu-id="7a256-102">Hello postup v tomto kurzu se předpokládá, že používáte Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="7a256-102">hello steps in this tutorial assume you are running Ubuntu Linux.</span></span>

<span data-ttu-id="7a256-103">Otevřete prostředí a spusťte následující příkazy tooinstall hello požadované balíčky hello:</span><span class="sxs-lookup"><span data-stu-id="7a256-103">Open a shell and run hello following commands tooinstall hello prerequisite packages:</span></span>

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

<span data-ttu-id="7a256-104">V prostředí hello spusťte následující příkaz tooclone hello Azure IoT Edge Githubu úložiště tooyour místního počítače hello:</span><span class="sxs-lookup"><span data-stu-id="7a256-104">In hello shell, run hello following command tooclone hello Azure IoT Edge GitHub repository tooyour local machine:</span></span>

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-toobuild-hello-sample"></a><span data-ttu-id="7a256-105">Jak toobuild hello ukázka</span><span class="sxs-lookup"><span data-stu-id="7a256-105">How toobuild hello sample</span></span>

<span data-ttu-id="7a256-106">Teď můžete sestavit hello IoT Edge runtime a ukázky na místním počítači:</span><span class="sxs-lookup"><span data-stu-id="7a256-106">You can now build hello IoT Edge runtime and samples on your local machine:</span></span>

1. <span data-ttu-id="7a256-107">Otevřete prostředí.</span><span class="sxs-lookup"><span data-stu-id="7a256-107">Open a shell.</span></span>

1. <span data-ttu-id="7a256-108">Přejděte toohello kořenovou složku v místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="7a256-108">Navigate toohello root folder in your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="7a256-109">Spusťte skript sestavení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7a256-109">Run the build script as follows:</span></span>

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

<span data-ttu-id="7a256-110">Tento skript používá **cmake** toocreate nástroj složky s názvem **sestavení** hello kořenové složky vaší místní kopii **iot hranou** úložiště a generování souboru pravidel.</span><span class="sxs-lookup"><span data-stu-id="7a256-110">This script uses the **cmake** utility toocreate a folder called **build** in hello root folder of your local copy of the **iot-edge** repository and generate a makefile.</span></span> <span data-ttu-id="7a256-111">Hello skript potom vytvoří hello řešení, přeskočení testy částí a end tooend testy.</span><span class="sxs-lookup"><span data-stu-id="7a256-111">hello script then builds hello solution, skipping unit tests and end tooend tests.</span></span> <span data-ttu-id="7a256-112">Pokud chcete toobuild a spouštění testování částí hello, přidejte hello `--run-unittests` parametr.</span><span class="sxs-lookup"><span data-stu-id="7a256-112">If you want toobuild and run hello unit tests, add hello `--run-unittests` parameter.</span></span> <span data-ttu-id="7a256-113">Pokud chcete toobuild a testy tooend hello end, přidejte hello `--run-e2e-tests`.</span><span class="sxs-lookup"><span data-stu-id="7a256-113">If you want toobuild and run hello end tooend tests, add hello `--run-e2e-tests`.</span></span>

> [!NOTE]
> <span data-ttu-id="7a256-114">Pokaždé, když spustíte hello **build.sh** skriptu, se odstraní a potom znovu vytvoří hello **sestavení** složky v kořenové složce hello místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="7a256-114">Every time you run hello **build.sh** script, it deletes and then recreates hello **build** folder in hello root folder of your local copy of hello **iot-edge** repository.</span></span>
