<span data-ttu-id="44306-101">Azure určí, že vaše aplikace používá Python, **pokud jsou splněné obě tyto podmínky**:</span><span class="sxs-lookup"><span data-stu-id="44306-101">Azure will determine that your application uses Python **if both of these conditions are true**:</span></span>

* <span data-ttu-id="44306-102">Soubor requirements.txt v kořenové složce</span><span class="sxs-lookup"><span data-stu-id="44306-102">requirements.txt file in the root folder</span></span>
* <span data-ttu-id="44306-103">Jakýkoli soubor .py v kořenové složce NEBO soubor runtime.txt, který určuje jazyk Python</span><span class="sxs-lookup"><span data-stu-id="44306-103">any .py file in the root folder OR a runtime.txt that specifies python</span></span>

<span data-ttu-id="44306-104">V takovém případě se použije konkrétní skript nasazení jazyka Python, který provede standardní synchronizaci souborů a další operace jazyka Python, jako jsou tyto:</span><span class="sxs-lookup"><span data-stu-id="44306-104">When that's the case, it will use a Python specific deployment script, which performs the standard synchronization of files, as well as additional Python operations such as:</span></span>

* <span data-ttu-id="44306-105">Automatická správa virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="44306-105">Automatic management of virtual environment</span></span>
* <span data-ttu-id="44306-106">Instalace balíčků uvedených v souboru requirements.txt pomocí nástroje pip</span><span class="sxs-lookup"><span data-stu-id="44306-106">Installation of packages listed in requirements.txt using pip</span></span>
* <span data-ttu-id="44306-107">Vytvoření odpovídajícího souboru web.config na základě vybrané verze jazyka Python.</span><span class="sxs-lookup"><span data-stu-id="44306-107">Creation of the appropriate web.config based on the selected Python version.</span></span>
* <span data-ttu-id="44306-108">Shromáždění statických souborů pro aplikace Django</span><span class="sxs-lookup"><span data-stu-id="44306-108">Collect static files for Django applications</span></span>

<span data-ttu-id="44306-109">Můžete ovládat některé aspekty výchozích kroků nasazení, aniž by bylo potřeba skript přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="44306-109">You can control certain aspects of the default deployment steps without having to customize the script.</span></span>

<span data-ttu-id="44306-110">Pokud chcete přeskočit všechny kroky nasazení specifické pro Python, můžete vytvořit tento prázdný soubor:</span><span class="sxs-lookup"><span data-stu-id="44306-110">If you want to skip all Python specific deployment steps, you can create this empty file:</span></span>

    \.skipPythonDeployment

<span data-ttu-id="44306-111">Pokud chcete mít větší kontrolu nad nasazením, můžete výchozí skript nasazení potlačit vytvořením následujících souborů:</span><span class="sxs-lookup"><span data-stu-id="44306-111">For more control over deployment, you can override the default deployment script by creating the following files:</span></span>

    \.deployment
    \deploy.cmd

<span data-ttu-id="44306-112">Můžete použít [rozhraní příkazového řádku Azure] [ Azure command-line interface] k vytvoření těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="44306-112">You can use the [Azure command-line interface][Azure command-line interface] to create the files.</span></span>  <span data-ttu-id="44306-113">Použijte tento příkaz ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="44306-113">Use this command from your project folder:</span></span>

    azure site deploymentscript --python

<span data-ttu-id="44306-114">Pokud tyto soubory neexistují, Azure vytvoří dočasný skript nasazení a spustí ho.</span><span class="sxs-lookup"><span data-stu-id="44306-114">When these files don't exist, Azure creates a temporary deployment script and runs it.</span></span>  <span data-ttu-id="44306-115">Je stejný jako ten, který vytvoříte pomocí výše uvedeného příkazu.</span><span class="sxs-lookup"><span data-stu-id="44306-115">It is identical to the one you create with the command above.</span></span>

[Azure command-line interface]: http://azure.microsoft.com/downloads/
