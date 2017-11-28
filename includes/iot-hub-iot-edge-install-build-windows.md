## <a name="install-the-prerequisites"></a><span data-ttu-id="0edb7-101">Nainstalujte součásti</span><span class="sxs-lookup"><span data-stu-id="0edb7-101">Install the prerequisites</span></span>

1. <span data-ttu-id="0edb7-102">Nainstalujte [Visual Studio 2015 nebo 2017](https://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="0edb7-102">Install [Visual Studio 2015 or 2017](https://www.visualstudio.com).</span></span> <span data-ttu-id="0edb7-103">Bezplatná edice Community můžete použít, pokud splňují požadavky na licencování.</span><span class="sxs-lookup"><span data-stu-id="0edb7-103">You can use the free Community Edition if you meet the licensing requirements.</span></span> <span data-ttu-id="0edb7-104">Nezapomeňte zahrnout Visual C++ a Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="0edb7-104">Be sure to include Visual C++ and NuGet Package Manager.</span></span>

1. <span data-ttu-id="0edb7-105">Nainstalujte [git](http://www.git-scm.com) a zajistěte, aby git.exe můžete spustit z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0edb7-105">Install [git](http://www.git-scm.com) and make sure you can run git.exe from the command line.</span></span>

1. <span data-ttu-id="0edb7-106">Nainstalujte [CMake](https://cmake.org/download/) a zajistěte, aby cmake.exe můžete spustit z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0edb7-106">Install [CMake](https://cmake.org/download/) and make sure you can run cmake.exe from the command line.</span></span> <span data-ttu-id="0edb7-107">CMake verze 3.7.2 nebo novější je doporučené.</span><span class="sxs-lookup"><span data-stu-id="0edb7-107">CMake version 3.7.2 or later is recommended.</span></span> <span data-ttu-id="0edb7-108">**.Msi** instalační program je nejjednodušší možnost v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="0edb7-108">The **.msi** installer is the easiest option on Windows.</span></span> <span data-ttu-id="0edb7-109">Přidání softwaru CMake k CESTĚ pro alespoň aktuálního uživatele, když instalační program zobrazí výzvu.</span><span class="sxs-lookup"><span data-stu-id="0edb7-109">Add CMake to the PATH for at least the current user when the installer prompts you.</span></span>

1. <span data-ttu-id="0edb7-110">Nainstalujte [Python 2.7](https://www.python.org/downloads/release/python-27).</span><span class="sxs-lookup"><span data-stu-id="0edb7-110">Install [Python 2.7](https://www.python.org/downloads/release/python-27).</span></span> <span data-ttu-id="0edb7-111">Ověřte, že je přidat Python pro vaše `PATH` proměnné prostředí v **ovládací panely -> Systém -> Upřesnit nastavení systému -> proměnné prostředí**.</span><span class="sxs-lookup"><span data-stu-id="0edb7-111">Make sure you add Python to your `PATH` environment variable in **Control Panel -> System -> Advanced system settings -> Environment Variables**.</span></span>

1. <span data-ttu-id="0edb7-112">Na příkazovém řádku spusťte následující příkaz, který naklonujte úložiště Azure IoT Edge GitHub do místního počítače:</span><span class="sxs-lookup"><span data-stu-id="0edb7-112">At a command prompt, run the following command to clone the Azure IoT Edge GitHub repository to your local machine:</span></span>

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-to-build-the-sample"></a><span data-ttu-id="0edb7-113">Postup pro sestavení ukázky</span><span class="sxs-lookup"><span data-stu-id="0edb7-113">How to build the sample</span></span>

<span data-ttu-id="0edb7-114">Teď můžete sestavit IoT Edge runtime a ukázky na místním počítači:</span><span class="sxs-lookup"><span data-stu-id="0edb7-114">You can now build the IoT Edge runtime and samples on your local machine:</span></span>

1. <span data-ttu-id="0edb7-115">Otevřete **příkazový řádek vývojáře pro VS 2015** nebo **příkazový řádek vývojáře pro VS 2017** příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0edb7-115">Open a **Developer Command Prompt for VS 2015** or **Developer Command Prompt for VS 2017** command prompt.</span></span>

1. <span data-ttu-id="0edb7-116">Přejděte do kořenové složky místní kopie úložiště **iot-edge**.</span><span class="sxs-lookup"><span data-stu-id="0edb7-116">Navigate to the root folder in your local copy of the **iot-edge** repository.</span></span>

1. <span data-ttu-id="0edb7-117">Spusťte skript sestavení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0edb7-117">Run the build script as follows:</span></span>

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

<span data-ttu-id="0edb7-118">Tento skript vytvoří soubor řešení sady Visual Studio a vytvoří řešení.</span><span class="sxs-lookup"><span data-stu-id="0edb7-118">This script creates a Visual Studio solution file and builds the solution.</span></span> <span data-ttu-id="0edb7-119">Můžete najít řešení v sadě Visual Studio **sestavení** složky ve vaší místní kopii **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="0edb7-119">You can find the Visual Studio solution in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="0edb7-120">Pokud chcete vytvořit a spouštění testování částí, přidat `--run-unittests` parametr.</span><span class="sxs-lookup"><span data-stu-id="0edb7-120">If you want to build and run the unit tests, add the `--run-unittests` parameter.</span></span> <span data-ttu-id="0edb7-121">Pokud chcete sestavit a spustit testy koncová, přidat `--run-e2e-tests`.</span><span class="sxs-lookup"><span data-stu-id="0edb7-121">If you want to build and run the end to end tests, add the `--run-e2e-tests`.</span></span>

> [!NOTE]
> <span data-ttu-id="0edb7-122">Pokaždé, když spustíte **build.cmd** skriptu, se odstraní a potom znovu vytvoří **sestavení** složky v kořenové složce místní kopii **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="0edb7-122">Every time you run the **build.cmd** script, it deletes and then recreates the **build** folder in the root folder of your local copy of the **iot-edge** repository.</span></span>