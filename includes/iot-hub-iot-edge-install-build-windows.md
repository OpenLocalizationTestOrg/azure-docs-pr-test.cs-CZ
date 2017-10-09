## <a name="install-hello-prerequisites"></a><span data-ttu-id="15c37-101">Instalace požadovaných součástí hello</span><span class="sxs-lookup"><span data-stu-id="15c37-101">Install hello prerequisites</span></span>

1. <span data-ttu-id="15c37-102">Nainstalujte [Visual Studio 2015 nebo 2017](https://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="15c37-102">Install [Visual Studio 2015 or 2017](https://www.visualstudio.com).</span></span> <span data-ttu-id="15c37-103">Můžete použít hello volné Community Edition, pokud splňují hello licenční požadavky.</span><span class="sxs-lookup"><span data-stu-id="15c37-103">You can use hello free Community Edition if you meet hello licensing requirements.</span></span> <span data-ttu-id="15c37-104">Být, že tooinclude Visual C++ a Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="15c37-104">Be sure tooinclude Visual C++ and NuGet Package Manager.</span></span>

1. <span data-ttu-id="15c37-105">Nainstalujte [git](http://www.git-scm.com) a zajistěte, aby git.exe můžete spustit z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="15c37-105">Install [git](http://www.git-scm.com) and make sure you can run git.exe from hello command line.</span></span>

1. <span data-ttu-id="15c37-106">Nainstalujte [CMake](https://cmake.org/download/) a zajistěte, aby cmake.exe můžete spustit z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="15c37-106">Install [CMake](https://cmake.org/download/) and make sure you can run cmake.exe from hello command line.</span></span> <span data-ttu-id="15c37-107">CMake verze 3.7.2 nebo novější je doporučené.</span><span class="sxs-lookup"><span data-stu-id="15c37-107">CMake version 3.7.2 or later is recommended.</span></span> <span data-ttu-id="15c37-108">Hello **.msi** instalační program je nejjednodušší možnost hello v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="15c37-108">hello **.msi** installer is hello easiest option on Windows.</span></span> <span data-ttu-id="15c37-109">Přidat CMake toohello CESTU pro alespoň hello aktuálního uživatele, když hello instalační program zobrazí výzvu.</span><span class="sxs-lookup"><span data-stu-id="15c37-109">Add CMake toohello PATH for at least hello current user when hello installer prompts you.</span></span>

1. <span data-ttu-id="15c37-110">Nainstalujte [Python 2.7](https://www.python.org/downloads/release/python-27).</span><span class="sxs-lookup"><span data-stu-id="15c37-110">Install [Python 2.7](https://www.python.org/downloads/release/python-27).</span></span> <span data-ttu-id="15c37-111">Zajistěte, aby přidáte Python tooyour `PATH` proměnné prostředí v **ovládací panely -> Systém -> Upřesnit nastavení systému -> proměnné prostředí**.</span><span class="sxs-lookup"><span data-stu-id="15c37-111">Make sure you add Python tooyour `PATH` environment variable in **Control Panel -> System -> Advanced system settings -> Environment Variables**.</span></span>

1. <span data-ttu-id="15c37-112">Na příkazovém řádku spusťte následující příkaz tooclone hello Azure IoT Edge Githubu úložiště tooyour místního počítače hello:</span><span class="sxs-lookup"><span data-stu-id="15c37-112">At a command prompt, run hello following command tooclone hello Azure IoT Edge GitHub repository tooyour local machine:</span></span>

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-toobuild-hello-sample"></a><span data-ttu-id="15c37-113">Jak toobuild hello ukázka</span><span class="sxs-lookup"><span data-stu-id="15c37-113">How toobuild hello sample</span></span>

<span data-ttu-id="15c37-114">Teď můžete sestavit hello IoT Edge runtime a ukázky na místním počítači:</span><span class="sxs-lookup"><span data-stu-id="15c37-114">You can now build hello IoT Edge runtime and samples on your local machine:</span></span>

1. <span data-ttu-id="15c37-115">Otevřete **příkazový řádek vývojáře pro VS 2015** nebo **příkazový řádek vývojáře pro VS 2017** příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="15c37-115">Open a **Developer Command Prompt for VS 2015** or **Developer Command Prompt for VS 2017** command prompt.</span></span>

1. <span data-ttu-id="15c37-116">Přejděte toohello kořenovou složku v místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="15c37-116">Navigate toohello root folder in your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="15c37-117">Spusťte skript sestavení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="15c37-117">Run the build script as follows:</span></span>

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

<span data-ttu-id="15c37-118">Tento skript vytvoří soubor řešení sady Visual Studio a vytvoří řešení hello.</span><span class="sxs-lookup"><span data-stu-id="15c37-118">This script creates a Visual Studio solution file and builds hello solution.</span></span> <span data-ttu-id="15c37-119">Hello řešení sady Visual Studio můžete najít v hello **sestavení** složky ve vaší místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="15c37-119">You can find hello Visual Studio solution in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="15c37-120">Pokud chcete toobuild a spouštění testování částí hello, přidejte hello `--run-unittests` parametr.</span><span class="sxs-lookup"><span data-stu-id="15c37-120">If you want toobuild and run hello unit tests, add hello `--run-unittests` parameter.</span></span> <span data-ttu-id="15c37-121">Pokud chcete toobuild a testy tooend hello end, přidejte hello `--run-e2e-tests`.</span><span class="sxs-lookup"><span data-stu-id="15c37-121">If you want toobuild and run hello end tooend tests, add hello `--run-e2e-tests`.</span></span>

> [!NOTE]
> <span data-ttu-id="15c37-122">Pokaždé, když spustíte hello **build.cmd** skriptu, se odstraní a potom znovu vytvoří hello **sestavení** složky v kořenové složce hello místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="15c37-122">Every time you run hello **build.cmd** script, it deletes and then recreates hello **build** folder in hello root folder of your local copy of hello **iot-edge** repository.</span></span>
