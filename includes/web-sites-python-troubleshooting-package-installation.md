<span data-ttu-id="0db9d-101">Některé balíčky se při spuštění v Azure nemusí pomocí systému pip nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="0db9d-101">Some packages may not install using pip when run on Azure.</span></span>  <span data-ttu-id="0db9d-102">Může být tento balíček hello není k dispozici na hello indexu balíčků Pythonu.</span><span class="sxs-lookup"><span data-stu-id="0db9d-102">It may simply be that hello package is not available on hello Python Package Index.</span></span>  <span data-ttu-id="0db9d-103">Může být, že je požadován kompilátor (kompilátor není k dispozici na hello počítač běžící hello webové aplikace v Azure App Service).</span><span class="sxs-lookup"><span data-stu-id="0db9d-103">It could be that a compiler is required (a compiler is not available on hello machine running hello web app in Azure App Service).</span></span>

<span data-ttu-id="0db9d-104">V této části se podíváme způsoby toodeal s tímto problémem.</span><span class="sxs-lookup"><span data-stu-id="0db9d-104">In this section, we'll look at ways toodeal with this issue.</span></span>

### <a name="request-wheels"></a><span data-ttu-id="0db9d-105">Vyžádání souborů wheel</span><span class="sxs-lookup"><span data-stu-id="0db9d-105">Request wheels</span></span>
<span data-ttu-id="0db9d-106">Pokud hello instalace balíčku vyžaduje kompilátor, pokuste se kontaktovat vlastníka toorequest balíček hello, která souborů Wheel dostupná pro balíček hello.</span><span class="sxs-lookup"><span data-stu-id="0db9d-106">If hello package installation requires a compiler, you should try contacting hello package owner toorequest that wheels be made available for hello package.</span></span>

<span data-ttu-id="0db9d-107">S nedávno vydanému hello [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], je teď jednodušší toobuild balíčky, které mají nativní kód pro Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="0db9d-107">With hello recent availability of [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], it is now easier toobuild packages that have native code for Python 2.7.</span></span>

### <a name="build-wheels-requires-windows"></a><span data-ttu-id="0db9d-108">Sestavení souborů wheel (vyžaduje Windows)</span><span class="sxs-lookup"><span data-stu-id="0db9d-108">Build wheels (requires Windows)</span></span>
<span data-ttu-id="0db9d-109">Poznámka: Pokud použijete tuto možnost, ujistěte se, že balíček hello toocompile pomocí prostředí Python, který odpovídá hello platformě, architektuře a verzi, která se používá v hello webové aplikace v Azure App Service (Windows/32-bit/2.7 nebo 3.4).</span><span class="sxs-lookup"><span data-stu-id="0db9d-109">Note: When using this option, make sure toocompile hello package using a Python environment that matches hello platform/architecture/version that is used on hello web app in Azure App Service (Windows/32-bit/2.7 or 3.4).</span></span>

<span data-ttu-id="0db9d-110">Pokud hello balíček nenainstaluje, protože vyžaduje kompilátor, můžete nainstalovat hello kompilátoru na místním počítači a sestavit wheel hello balíčku, který potom zahrnete do úložiště.</span><span class="sxs-lookup"><span data-stu-id="0db9d-110">If hello package doesn't install because it requires a compiler, you can install hello compiler on your local machine and build a wheel for hello package, which you will then include in your repository.</span></span>

<span data-ttu-id="0db9d-111">Uživatelé Mac/Linux: Pokud nemáte počítač Windows tooa přístup, zobrazit [vytvoření virtuálního počítače s Windows] [ Create a Virtual Machine Running Windows] jak toocreate virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="0db9d-111">Mac/Linux Users: If you don't have access tooa Windows machine, see [Create a Virtual Machine Running Windows][Create a Virtual Machine Running Windows] for how toocreate a VM on Azure.</span></span>  <span data-ttu-id="0db9d-112">Můžete použít souborů Wheel toobuild hello, přidejte je toohello úložiště a pokud chcete zahodit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0db9d-112">You can use it toobuild hello wheels, add them toohello repository, and discard hello VM if you like.</span></span> 

<span data-ttu-id="0db9d-113">Python 2.7 můžete nainstalovat [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].</span><span class="sxs-lookup"><span data-stu-id="0db9d-113">For Python 2.7, you can install [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].</span></span>

<span data-ttu-id="0db9d-114">Pro Python 3.4 můžete nainstalovat [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].</span><span class="sxs-lookup"><span data-stu-id="0db9d-114">For Python 3.4, you can install [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].</span></span>

<span data-ttu-id="0db9d-115">toobuild souborů Wheel, budete potřebovat balíček wheel hello:</span><span class="sxs-lookup"><span data-stu-id="0db9d-115">toobuild wheels, you'll need hello wheel package:</span></span>

    env\scripts\pip install wheel

<span data-ttu-id="0db9d-116">Budete používat `pip wheel` toocompile závislost:</span><span class="sxs-lookup"><span data-stu-id="0db9d-116">You'll use `pip wheel` toocompile a dependency:</span></span>

    env\scripts\pip wheel azure==0.8.4

<span data-ttu-id="0db9d-117">Tím se vytvoří soubor .whl ve složce \wheelhouse hello.</span><span class="sxs-lookup"><span data-stu-id="0db9d-117">This creates a .whl file in hello \wheelhouse folder.</span></span>  <span data-ttu-id="0db9d-118">Přidáte složku \wheelhouse hello a kolečka soubory tooyour úložiště.</span><span class="sxs-lookup"><span data-stu-id="0db9d-118">Add hello \wheelhouse folder and wheel files tooyour repository.</span></span>

<span data-ttu-id="0db9d-119">Upravit vaše hello tooadd requirements.txt `--find-links` možnost v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="0db9d-119">Edit your requirements.txt tooadd hello `--find-links` option at hello top.</span></span> <span data-ttu-id="0db9d-120">Tato hodnota informuje pip toolook pro přesnou shodu v místní složce hello před indexu balíčků pythonu toohello probíhající.</span><span class="sxs-lookup"><span data-stu-id="0db9d-120">This tells pip toolook for an exact match in hello local folder before going toohello python package index.</span></span>

    --find-links wheelhouse
    azure==0.8.4

<span data-ttu-id="0db9d-121">Pokud chcete tooinclude všechny svoje závislosti hello \wheelhouse složku a nepoužije hello python package index vůbec, můžete vynutit indexu balíčků hello tooignore pip přidáním `--no-index` toohello začátek souboru requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="0db9d-121">If you want tooinclude all your dependencies in hello \wheelhouse folder and not use hello python package index at all, you can force pip tooignore hello package index by adding `--no-index` toohello top of your requirements.txt.</span></span>

    --no-index

### <a name="customize-installation"></a><span data-ttu-id="0db9d-122">Přizpůsobení instalace</span><span class="sxs-lookup"><span data-stu-id="0db9d-122">Customize installation</span></span>
<span data-ttu-id="0db9d-123">Můžete přizpůsobit hello nasazení skriptu tooinstall balíček ve virtuálním prostředí hello pomocí alternativního instalačního programu, například easy\_nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="0db9d-123">You can customize hello deployment script tooinstall a package in hello virtual environment using an alternate installer, such as easy\_install.</span></span>  <span data-ttu-id="0db9d-124">Příklad tohoto postupu označený jako komentář najdete v souboru deploy.cmd.  Ujistěte se, že tyto balíčky nejsou uvedené v souboru requirements.txt, tooprevent pip instalaci je.</span><span class="sxs-lookup"><span data-stu-id="0db9d-124">See deploy.cmd for an example that is commented out.  Make sure that such packages aren't listed in requirements.txt, tooprevent pip from installing them.</span></span>

<span data-ttu-id="0db9d-125">Přidejte tento skript nasazení toohello:</span><span class="sxs-lookup"><span data-stu-id="0db9d-125">Add this toohello deployment script:</span></span>

    env\scripts\easy_install somepackage

<span data-ttu-id="0db9d-126">Může být také možné toouse snadno\_tooinstall nainstalovat z instalačního programu exe (některé jsou zip kompatibilní, takže je easy\_install podporuje).</span><span class="sxs-lookup"><span data-stu-id="0db9d-126">You may also be able toouse easy\_install tooinstall from an exe installer (some are zip compatible, so easy\_install supports them).</span></span>  <span data-ttu-id="0db9d-127">Přidejte úložiště tooyour hello instalačního programu a spusťte easy\_nainstalovat předáním toohello hello cestu spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="0db9d-127">Add hello installer tooyour repository, and invoke easy\_install by passing hello path toohello executable.</span></span>

<span data-ttu-id="0db9d-128">Přidejte tento skript nasazení toohello:</span><span class="sxs-lookup"><span data-stu-id="0db9d-128">Add this toohello deployment script:</span></span>

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-hello-virtual-environment-in-hello-repository-requires-windows"></a><span data-ttu-id="0db9d-129">Zahrnout virtuální prostředí hello hello úložiště (vyžaduje Windows)</span><span class="sxs-lookup"><span data-stu-id="0db9d-129">Include hello virtual environment in hello repository (requires Windows)</span></span>
<span data-ttu-id="0db9d-130">Poznámka: Pokud použijete tuto možnost, ujistěte se, že toouse virtuální prostředí, které odpovídá hello platformě, architektuře a verzi, která se používá v hello webové aplikace v Azure App Service (Windows/32-bit/2.7 nebo 3.4).</span><span class="sxs-lookup"><span data-stu-id="0db9d-130">Note: When using this option, make sure toouse a virtual environment that matches hello platform/architecture/version that is used on hello web app in Azure App Service (Windows/32-bit/2.7 or 3.4).</span></span>

<span data-ttu-id="0db9d-131">Pokud zahrnete virtuální prostředí hello hello úložiště, můžete skript nasazení hello zabránit správě virtuálního prostředí v Azure vytvořením prázdného souboru:</span><span class="sxs-lookup"><span data-stu-id="0db9d-131">If you include hello virtual environment in hello repository, you can prevent hello deployment script from doing virtual environment management on Azure by creating an empty file:</span></span>

    .skipPythonDeployment

<span data-ttu-id="0db9d-132">Doporučujeme odstranit existující virtuální prostředí v aplikaci hello zanechání nepotřebných souborů z tooprevent hello, pokud bylo hello virtuální prostředí spravováno automaticky.</span><span class="sxs-lookup"><span data-stu-id="0db9d-132">We recommend that you delete hello existing virtual environment on hello app, tooprevent leftover files from when hello virtual environment was managed automatically.</span></span>

[Create a Virtual Machine Running Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ Compiler for Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
