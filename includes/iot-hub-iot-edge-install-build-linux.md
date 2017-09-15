## <a name="install-the-prerequisites"></a><span data-ttu-id="de204-101">Nainstalujte součásti</span><span class="sxs-lookup"><span data-stu-id="de204-101">Install the prerequisites</span></span>

<span data-ttu-id="de204-102">Postup v tomto kurzu se předpokládá, že používáte Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="de204-102">The steps in this tutorial assume you are running Ubuntu Linux.</span></span>

<span data-ttu-id="de204-103">Otevřete prostředí a spusťte následující příkazy pro instalaci požadovaných balíčků:</span><span class="sxs-lookup"><span data-stu-id="de204-103">Open a shell and run the following commands to install the prerequisite packages:</span></span>

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

<span data-ttu-id="de204-104">V prostředí spusťte následující příkaz, který naklonujte úložiště Azure IoT Edge GitHub do místního počítače:</span><span class="sxs-lookup"><span data-stu-id="de204-104">In the shell, run the following command to clone the Azure IoT Edge GitHub repository to your local machine:</span></span>

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-to-build-the-sample"></a><span data-ttu-id="de204-105">Postup pro sestavení ukázky</span><span class="sxs-lookup"><span data-stu-id="de204-105">How to build the sample</span></span>

<span data-ttu-id="de204-106">Teď můžete sestavit IoT Edge runtime a ukázky na místním počítači:</span><span class="sxs-lookup"><span data-stu-id="de204-106">You can now build the IoT Edge runtime and samples on your local machine:</span></span>

1. <span data-ttu-id="de204-107">Otevřete prostředí.</span><span class="sxs-lookup"><span data-stu-id="de204-107">Open a shell.</span></span>

1. <span data-ttu-id="de204-108">Přejděte do kořenové složky místní kopie úložiště **iot-edge**.</span><span class="sxs-lookup"><span data-stu-id="de204-108">Navigate to the root folder in your local copy of the **iot-edge** repository.</span></span>

1. <span data-ttu-id="de204-109">Spusťte skript sestavení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="de204-109">Run the build script as follows:</span></span>

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

<span data-ttu-id="de204-110">Tento skript vytvoří v kořenové složce místní kopie úložiště **iot-edge** pomocí nástroje **cmake** složku **build** a vygeneruje soubor pravidel.</span><span class="sxs-lookup"><span data-stu-id="de204-110">This script uses the **cmake** utility to create a folder called **build** in the root folder of your local copy of the **iot-edge** repository and generate a makefile.</span></span> <span data-ttu-id="de204-111">Skript potom sestaví řešení a přeskočí při tom testy jednotek a celkové testy.</span><span class="sxs-lookup"><span data-stu-id="de204-111">The script then builds the solution, skipping unit tests and end to end tests.</span></span> <span data-ttu-id="de204-112">Pokud chcete vytvořit a spouštění testování částí, přidat `--run-unittests` parametr.</span><span class="sxs-lookup"><span data-stu-id="de204-112">If you want to build and run the unit tests, add the `--run-unittests` parameter.</span></span> <span data-ttu-id="de204-113">Pokud chcete sestavit a spustit testy koncová, přidat `--run-e2e-tests`.</span><span class="sxs-lookup"><span data-stu-id="de204-113">If you want to build and run the end to end tests, add the `--run-e2e-tests`.</span></span>

> [!NOTE]
> <span data-ttu-id="de204-114">Pokaždé když spustíte skript **build.sh**, odstraní a potom znovu vytvoří složku **build** v kořenové složce místní kopie úložiště **iot-edge**.</span><span class="sxs-lookup"><span data-stu-id="de204-114">Every time you run the **build.sh** script, it deletes and then recreates the **build** folder in the root folder of your local copy of the **iot-edge** repository.</span></span>