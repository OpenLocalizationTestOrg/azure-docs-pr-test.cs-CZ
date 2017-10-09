<span data-ttu-id="20822-101">Azure určí, že vaše aplikace používá Python, **pokud jsou splněné obě tyto podmínky**:</span><span class="sxs-lookup"><span data-stu-id="20822-101">Azure will determine that your application uses Python **if both of these conditions are true**:</span></span>

* <span data-ttu-id="20822-102">soubor Requirements.txt v kořenové složce hello</span><span class="sxs-lookup"><span data-stu-id="20822-102">requirements.txt file in hello root folder</span></span>
* <span data-ttu-id="20822-103">jakýkoli soubor .py v kořenové složce hello nebo soubor runtime.txt, který určuje jazyk python</span><span class="sxs-lookup"><span data-stu-id="20822-103">any .py file in hello root folder OR a runtime.txt that specifies python</span></span>

<span data-ttu-id="20822-104">Když, je případ hello, použije skript konkrétní nasazení, Python, který provede standardní synchronizaci hello soubory, jakož i další operace jazyka Python, jako:</span><span class="sxs-lookup"><span data-stu-id="20822-104">When that's hello case, it will use a Python specific deployment script, which performs hello standard synchronization of files, as well as additional Python operations such as:</span></span>

* <span data-ttu-id="20822-105">Automatická správa virtuálního prostředí</span><span class="sxs-lookup"><span data-stu-id="20822-105">Automatic management of virtual environment</span></span>
* <span data-ttu-id="20822-106">Instalace balíčků uvedených v souboru requirements.txt pomocí nástroje pip</span><span class="sxs-lookup"><span data-stu-id="20822-106">Installation of packages listed in requirements.txt using pip</span></span>
* <span data-ttu-id="20822-107">Vytvoření hello odpovídající souboru web.config podle hello vybraná verze Python.</span><span class="sxs-lookup"><span data-stu-id="20822-107">Creation of hello appropriate web.config based on hello selected Python version.</span></span>
* <span data-ttu-id="20822-108">Shromáždění statických souborů pro aplikace Django</span><span class="sxs-lookup"><span data-stu-id="20822-108">Collect static files for Django applications</span></span>

<span data-ttu-id="20822-109">Můžete řídit některé aspekty kroky nasazení výchozí hello bez nutnosti toocustomize hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="20822-109">You can control certain aspects of hello default deployment steps without having toocustomize hello script.</span></span>

<span data-ttu-id="20822-110">Pokud chcete tooskip všechny kroky nasazení specifické pro Python, můžete vytvořit tento prázdný soubor:</span><span class="sxs-lookup"><span data-stu-id="20822-110">If you want tooskip all Python specific deployment steps, you can create this empty file:</span></span>

    \.skipPythonDeployment

<span data-ttu-id="20822-111">Pro větší kontrolu nad nasazením můžete přepsat výchozí skript nasazení hello vytvořením hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="20822-111">For more control over deployment, you can override hello default deployment script by creating hello following files:</span></span>

    \.deployment
    \deploy.cmd

<span data-ttu-id="20822-112">Můžete použít hello [rozhraní příkazového řádku Azure] [ Azure command-line interface] toocreate hello soubory.</span><span class="sxs-lookup"><span data-stu-id="20822-112">You can use hello [Azure command-line interface][Azure command-line interface] toocreate hello files.</span></span>  <span data-ttu-id="20822-113">Použijte tento příkaz ze složky projektu:</span><span class="sxs-lookup"><span data-stu-id="20822-113">Use this command from your project folder:</span></span>

    azure site deploymentscript --python

<span data-ttu-id="20822-114">Pokud tyto soubory neexistují, Azure vytvoří dočasný skript nasazení a spustí ho.</span><span class="sxs-lookup"><span data-stu-id="20822-114">When these files don't exist, Azure creates a temporary deployment script and runs it.</span></span>  <span data-ttu-id="20822-115">Je identické toohello jeden vytvoříte příkazem hello výše.</span><span class="sxs-lookup"><span data-stu-id="20822-115">It is identical toohello one you create with hello command above.</span></span>

[Azure command-line interface]: http://azure.microsoft.com/downloads/
