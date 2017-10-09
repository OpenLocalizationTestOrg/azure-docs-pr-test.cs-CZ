<span data-ttu-id="79a9d-101">Vytvořit přihlašovací údaje nasazení s hello [nastavený uživatel nasazení webapp az](/cli/azure/webapp/deployment/user#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="79a9d-101">Create deployment credentials with hello [az webapp deployment user set](/cli/azure/webapp/deployment/user#set) command.</span></span>

<span data-ttu-id="79a9d-102">Uživatel nasazení je požadovaná pro FTP a místní Git nasazení tooa webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="79a9d-102">A deployment user is required for FTP and local Git deployment tooa web app.</span></span> <span data-ttu-id="79a9d-103">Hello uživatelské jméno a heslo jsou na úrovni účtu.</span><span class="sxs-lookup"><span data-stu-id="79a9d-103">hello user name and password are account level.</span></span> <span data-ttu-id="79a9d-104">_To znamená, že se liší od přihlašovacích údajů předplatného Azure._</span><span class="sxs-lookup"><span data-stu-id="79a9d-104">_They are different from your Azure subscription credentials._</span></span>

<span data-ttu-id="79a9d-105">Následující příkaz a nahraďte v hello  *\<uživatelské jméno >* a  *\<heslo >* s nové uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="79a9d-105">In hello following command, replace *\<user-name>* and *\<password>* with a new user name and password.</span></span> <span data-ttu-id="79a9d-106">Hello uživatelské jméno musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="79a9d-106">hello user name must be unique.</span></span> <span data-ttu-id="79a9d-107">Hello heslo musí obsahovat alespoň osm znaků, se dvěma hello následující tři prvky: písmena, symboly a čísla.</span><span class="sxs-lookup"><span data-stu-id="79a9d-107">hello password must be at least eight characters long, with two of hello following three elements: letters, numbers, symbols.</span></span> 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

<span data-ttu-id="79a9d-108">Pokud dojde ` 'Conflict'. Details: 409` chyby, uživatelské jméno změnit hello.</span><span class="sxs-lookup"><span data-stu-id="79a9d-108">If you get a ` 'Conflict'. Details: 409` error, change hello username.</span></span> <span data-ttu-id="79a9d-109">Pokud se zobrazí chyba ` 'Bad Request'. Details: 400`, použijte silnější heslo.</span><span class="sxs-lookup"><span data-stu-id="79a9d-109">If you get a ` 'Bad Request'. Details: 400` error, use a stronger password.</span></span>

<span data-ttu-id="79a9d-110">Tohoto uživatele nasazení vytvoříte jenom jednou a potom ho můžete použít pro všechna nasazení v Azure.</span><span class="sxs-lookup"><span data-stu-id="79a9d-110">You create this deployment user only once; you can use it for all your Azure deployments.</span></span>

> [!NOTE]
> <span data-ttu-id="79a9d-111">Záznam hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="79a9d-111">Record hello user name and password.</span></span> <span data-ttu-id="79a9d-112">Budete je potřebovat toodeploy hello webovou aplikaci později.</span><span class="sxs-lookup"><span data-stu-id="79a9d-112">You need them toodeploy hello web app later.</span></span>
>
>