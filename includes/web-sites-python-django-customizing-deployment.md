<span data-ttu-id="a1c89-101">Azure určí, že vaše aplikace používá Python, **pokud jsou splněné obě tyto podmínky**:</span><span class="sxs-lookup"><span data-stu-id="a1c89-101">Azure will determine that your application uses Python **if both of these conditions are true**:</span></span>

* <span data-ttu-id="a1c89-102">Soubor requirements.txt v kořenové složce</span><span class="sxs-lookup"><span data-stu-id="a1c89-102">requirements.txt file in the root folder</span></span>
* <span data-ttu-id="a1c89-103">Jakýkoli soubor .py v kořenové složce NEBO soubor runtime.txt, který určuje jazyk Python</span><span class="sxs-lookup"><span data-stu-id="a1c89-103">any .py file in the root folder OR a runtime.txt that specifies python</span></span>

<span data-ttu-id="a1c89-104">V takovém případě se použije konkrétní skript nasazení jazyka Python, který provede standardní synchronizaci souborů a další operace jazyka Python, jako jsou tyto:</span><span class="sxs-lookup"><span data-stu-id="a1c89-104">When that's the case, it will use a Python specific deployment script, which performs the standard synchronization of files, as well as additional Python operations such as:</span></span>

* <span data-ttu-id="a1c89-105">Automatická správa virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="a1c89-105">Automatic management of virtual environment</span></span>
* <span data-ttu-id="a1c89-106">Instalace balíčků uvedených v souboru requirements.txt pomocí nástroje pip</span><span class="sxs-lookup"><span data-stu-id="a1c89-106">Installation of packages listed in requirements.txt using pip</span></span>
* <span data-ttu-id="a1c89-107">Vytvoření odpovídajícího souboru web.config na základě vybrané verze jazyka Python.</span><span class="sxs-lookup"><span data-stu-id="a1c89-107">Creation of the appropriate web.config based on the selected Python version.</span></span>
* <span data-ttu-id="a1c89-108">Shromáždění statických souborů pro aplikace Django</span><span class="sxs-lookup"><span data-stu-id="a1c89-108">Collect static files for Django applications</span></span>

<span data-ttu-id="a1c89-109">Můžete ovládat některé aspekty výchozích kroků nasazení, aniž by bylo potřeba skript přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="a1c89-109">You can control certain aspects of the default deployment steps without having to customize the script.</span></span>

<span data-ttu-id="a1c89-110">Pokud chcete přeskočit všechny kroky nasazení specifické pro Python, můžete vytvořit tento prázdný soubor:</span><span class="sxs-lookup"><span data-stu-id="a1c89-110">If you want to skip all Python specific deployment steps, you can create this empty file:</span></span>

    \.skipPythonDeployment

<span data-ttu-id="a1c89-111">Chcete-li u aplikace rozhraní Django přeskočit shromažďování statických souborů:</span><span class="sxs-lookup"><span data-stu-id="a1c89-111">If you want to skip collection of static files for your Django application:</span></span>

    \.skipDjango 

<span data-ttu-id="a1c89-112">Pokud chcete mít větší kontrolu nad nasazením, můžete výchozí skript nasazení potlačit vytvořením následujících souborů:</span><span class="sxs-lookup"><span data-stu-id="a1c89-112">For more control over deployment, you can override the default deployment script by creating the following files:</span></span>

    \.deployment
    \deploy.cmd

<span data-ttu-id="a1c89-113">Můžete použít [rozhraní příkazového řádku Azure] [ Azure command-line interface] k vytvoření těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="a1c89-113">You can use the [Azure command-line interface][Azure command-line interface] to create the files.</span></span>  <span data-ttu-id="a1c89-114">Použijte tento příkaz ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="a1c89-114">Use this command from your project folder:</span></span>

    azure site deploymentscript --python

<span data-ttu-id="a1c89-115">Pokud tyto soubory neexistují, Azure vytvoří dočasný skript nasazení a spustí ho.</span><span class="sxs-lookup"><span data-stu-id="a1c89-115">When these files don't exist, Azure creates a temporary deployment script and runs it.</span></span>  <span data-ttu-id="a1c89-116">Je stejný jako ten, který vytvoříte pomocí výše uvedeného příkazu.</span><span class="sxs-lookup"><span data-stu-id="a1c89-116">It is identical to the one you create with the command above.</span></span>

[Azure command-line interface]: http://azure.microsoft.com/downloads/
