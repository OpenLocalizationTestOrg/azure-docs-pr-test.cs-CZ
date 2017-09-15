<span data-ttu-id="96619-101">Některé balíčky se při spuštění v Azure nemusí pomocí systému pip nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="96619-101">Some packages may not install using pip when run on Azure.</span></span>  <span data-ttu-id="96619-102">Může to být jednoduše proto, že nejsou dostupné v indexu balíčků Pythonu.</span><span class="sxs-lookup"><span data-stu-id="96619-102">It may simply be that the package is not available on the Python Package Index.</span></span>  <span data-ttu-id="96619-103">Důvodem může být také to, že je požadován kompilátor (kompilátor není dostupný v počítači, ve kterém běží webová aplikace ve službě Azure App Service).</span><span class="sxs-lookup"><span data-stu-id="96619-103">It could be that a compiler is required (a compiler is not available on the machine running the web app in Azure App Service).</span></span>

<span data-ttu-id="96619-104">V této části najdete popis způsobů řešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="96619-104">In this section, we'll look at ways to deal with this issue.</span></span>

### <a name="request-wheels"></a><span data-ttu-id="96619-105">Vyžádání souborů wheel</span><span class="sxs-lookup"><span data-stu-id="96619-105">Request wheels</span></span>
<span data-ttu-id="96619-106">Pokud instalace balíčku vyžaduje kompilátor, pokuste se kontaktovat vlastníka balíčku a požádejte ho o zpřístupnění souborů wheel balíčku.</span><span class="sxs-lookup"><span data-stu-id="96619-106">If the package installation requires a compiler, you should try contacting the package owner to request that wheels be made available for the package.</span></span>

<span data-ttu-id="96619-107">S nedávno vydanému [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], je nyní snazší sestavovat balíčky, které mají nativní kód pro Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="96619-107">With the recent availability of [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], it is now easier to build packages that have native code for Python 2.7.</span></span>

### <a name="build-wheels-requires-windows"></a><span data-ttu-id="96619-108">Sestavení souborů wheel (vyžaduje Windows)</span><span class="sxs-lookup"><span data-stu-id="96619-108">Build wheels (requires Windows)</span></span>
<span data-ttu-id="96619-109">Poznámka: Při použití této možnosti je nutné balíček zkompilovat pomocí prostředí Python, které odpovídá platformě, architektuře a verzi použité ve webové aplikaci ve službě Azure App Service (Windows/32-bit/2.7 nebo 3.4).</span><span class="sxs-lookup"><span data-stu-id="96619-109">Note: When using this option, make sure to compile the package using a Python environment that matches the platform/architecture/version that is used on the web app in Azure App Service (Windows/32-bit/2.7 or 3.4).</span></span>

<span data-ttu-id="96619-110">Pokud se balíček nenainstaluje, protože vyžaduje kompilátor, můžete kompilátor nainstalovat na místním počítači a sestavit pro balíček soubor wheel, který potom zahrnete do úložiště.</span><span class="sxs-lookup"><span data-stu-id="96619-110">If the package doesn't install because it requires a compiler, you can install the compiler on your local machine and build a wheel for the package, which you will then include in your repository.</span></span>

<span data-ttu-id="96619-111">Uživatelé Mac/Linux: Pokud nemáte přístup k počítači s Windows, přečtěte si téma [vytvoření virtuálního počítače s Windows] [ Create a Virtual Machine Running Windows] pro vytvoření virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="96619-111">Mac/Linux Users: If you don't have access to a Windows machine, see [Create a Virtual Machine Running Windows][Create a Virtual Machine Running Windows] for how to create a VM on Azure.</span></span>  <span data-ttu-id="96619-112">Můžete ho použít k vytvoření souborů wheel, přidat je do úložiště a virtuální počítač zahodit.</span><span class="sxs-lookup"><span data-stu-id="96619-112">You can use it to build the wheels, add them to the repository, and discard the VM if you like.</span></span> 

<span data-ttu-id="96619-113">Python 2.7 můžete nainstalovat [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].</span><span class="sxs-lookup"><span data-stu-id="96619-113">For Python 2.7, you can install [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].</span></span>

<span data-ttu-id="96619-114">Pro Python 3.4 můžete nainstalovat [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].</span><span class="sxs-lookup"><span data-stu-id="96619-114">For Python 3.4, you can install [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].</span></span>

<span data-ttu-id="96619-115">K sestavení souborů wheel budete potřebovat balíček wheel:</span><span class="sxs-lookup"><span data-stu-id="96619-115">To build wheels, you'll need the wheel package:</span></span>

    env\scripts\pip install wheel

<span data-ttu-id="96619-116">Závislost zkompilujete pomocí příkazu `pip wheel`:</span><span class="sxs-lookup"><span data-stu-id="96619-116">You'll use `pip wheel` to compile a dependency:</span></span>

    env\scripts\pip wheel azure==0.8.4

<span data-ttu-id="96619-117">Vytvoříte tak soubor .whl ve složce \wheelhouse.</span><span class="sxs-lookup"><span data-stu-id="96619-117">This creates a .whl file in the \wheelhouse folder.</span></span>  <span data-ttu-id="96619-118">Složku \wheelhouse a soubory wheel přidejte do úložiště.</span><span class="sxs-lookup"><span data-stu-id="96619-118">Add the \wheelhouse folder and wheel files to your repository.</span></span>

<span data-ttu-id="96619-119">Upravte soubor requirements.txt a přidejte na jeho začátek možnost `--find-links`.</span><span class="sxs-lookup"><span data-stu-id="96619-119">Edit your requirements.txt to add the `--find-links` option at the top.</span></span> <span data-ttu-id="96619-120">Tento parametr řekne systému pip, aby před použitím indexu balíčků Pythonu nejprve vyhledal přesnou shodu v místní složce.</span><span class="sxs-lookup"><span data-stu-id="96619-120">This tells pip to look for an exact match in the local folder before going to the python package index.</span></span>

    --find-links wheelhouse
    azure==0.8.4

<span data-ttu-id="96619-121">Pokud chcete zahrnout všechny svoje závislosti do složky \wheelhouse, a index balíčků Pythonu vůbec nechcete použít, můžete přidáním parametru `--no-index` na začátek souboru requirements.txt vynutit, aby systém pip index balíčků ignoroval.</span><span class="sxs-lookup"><span data-stu-id="96619-121">If you want to include all your dependencies in the \wheelhouse folder and not use the python package index at all, you can force pip to ignore the package index by adding `--no-index` to the top of your requirements.txt.</span></span>

    --no-index

### <a name="customize-installation"></a><span data-ttu-id="96619-122">Přizpůsobení instalace</span><span class="sxs-lookup"><span data-stu-id="96619-122">Customize installation</span></span>
<span data-ttu-id="96619-123">Skript nasazení můžete přizpůsobit tak, aby nainstalovat balíček ve virtuálním prostředí pomocí alternativního instalačního programu, jako je například easy\_install.</span><span class="sxs-lookup"><span data-stu-id="96619-123">You can customize the deployment script to install a package in the virtual environment using an alternate installer, such as easy\_install.</span></span>  <span data-ttu-id="96619-124">Příklad tohoto postupu označený jako komentář najdete v souboru deploy.cmd.</span><span class="sxs-lookup"><span data-stu-id="96619-124">See deploy.cmd for an example that is commented out.</span></span>  <span data-ttu-id="96619-125">Ujistěte se, že tyto balíčky nejsou uvedené v souboru requirements.txt, abyste systému pip zabránili v jejich instalaci.</span><span class="sxs-lookup"><span data-stu-id="96619-125">Make sure that such packages aren't listed in requirements.txt, to prevent pip from installing them.</span></span>

<span data-ttu-id="96619-126">Do skriptu nasazení přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="96619-126">Add this to the deployment script:</span></span>

    env\scripts\easy_install somepackage

<span data-ttu-id="96619-127">V některých případech můžete k instalaci z instalačního souboru .exe použít také instalační program easy\_install (některé balíčky jsou kompatibilní s formátem .zip, takže je easy\_install podporuje).</span><span class="sxs-lookup"><span data-stu-id="96619-127">You may also be able to use easy\_install to install from an exe installer (some are zip compatible, so easy\_install supports them).</span></span>  <span data-ttu-id="96619-128">Přidejte instalační program do úložiště a předáním cesty spustitelnému souboru spusťte easy\_install.</span><span class="sxs-lookup"><span data-stu-id="96619-128">Add the installer to your repository, and invoke easy\_install by passing the path to the executable.</span></span>

<span data-ttu-id="96619-129">Do skriptu nasazení přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="96619-129">Add this to the deployment script:</span></span>

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a><span data-ttu-id="96619-130">Zahrnutí virtuálního prostředí do úložiště (vyžaduje Windows)</span><span class="sxs-lookup"><span data-stu-id="96619-130">Include the virtual environment in the repository (requires Windows)</span></span>
<span data-ttu-id="96619-131">Poznámka: Při použití této možnosti je nutné použít virtuální prostředí, které odpovídá platformě, architektuře a verzi použité ve webové aplikaci ve službě Azure App Service (Windows/32-bit/2.7 nebo 3.4).</span><span class="sxs-lookup"><span data-stu-id="96619-131">Note: When using this option, make sure to use a virtual environment that matches the platform/architecture/version that is used on the web app in Azure App Service (Windows/32-bit/2.7 or 3.4).</span></span>

<span data-ttu-id="96619-132">Pokud do úložiště zahrnete virtuální prostředí, můžete skriptu nasazení zabránit ve správě virtuálního prostředí v Azure vytvořením prázdného souboru:</span><span class="sxs-lookup"><span data-stu-id="96619-132">If you include the virtual environment in the repository, you can prevent the deployment script from doing virtual environment management on Azure by creating an empty file:</span></span>

    .skipPythonDeployment

<span data-ttu-id="96619-133">Existující virtuální prostředí v aplikaci se doporučuje odstranit, abyste zabránili zanechání nepotřebných souborů z období, kdy bylo virtuální prostředí spravováno automaticky.</span><span class="sxs-lookup"><span data-stu-id="96619-133">We recommend that you delete the existing virtual environment on the app, to prevent leftover files from when the virtual environment was managed automatically.</span></span>

[Create a Virtual Machine Running Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ Compiler for Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
