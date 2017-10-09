<span data-ttu-id="40702-101">Každý objekt blob v úložišti Azure se musí nacházet v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="40702-101">Every blob in Azure storage must reside in a container.</span></span> <span data-ttu-id="40702-102">Hello kontejner je součástí hello název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="40702-102">hello container forms part of hello blob name.</span></span> <span data-ttu-id="40702-103">Například `mycontainer` je název hello hello kontejneru v těchto ukázkových objektů blob identifikátory URI:</span><span class="sxs-lookup"><span data-stu-id="40702-103">For example, `mycontainer` is hello name of hello container in these sample blob URIs:</span></span>

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

<span data-ttu-id="40702-104">Název kontejneru musí být platný název DNS, vyhovující toohello následující pravidla pojmenování:</span><span class="sxs-lookup"><span data-stu-id="40702-104">A container name must be a valid DNS name, conforming toohello following naming rules:</span></span>

1. <span data-ttu-id="40702-105">Názvy kontejnerů musí začínat písmenem nebo číslicí a může obsahovat pouze písmena, číslice a pomlčky (-) znak hello.</span><span class="sxs-lookup"><span data-stu-id="40702-105">Container names must start with a letter or number, and can contain only letters, numbers, and hello dash (-) character.</span></span>
2. <span data-ttu-id="40702-106">Bezprostředně před a za každým znakem pomlčky (-) se musí nacházet písmeno nebo číslice. Názvy kontejnerů nesmí obsahovat po sobě jdoucí pomlčky.</span><span class="sxs-lookup"><span data-stu-id="40702-106">Every dash (-) character must be immediately preceded and followed by a letter or number; consecutive dashes are not permitted in container names.</span></span>
3. <span data-ttu-id="40702-107">Název kontejneru musí být psaný malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="40702-107">All letters in a container name must be lowercase.</span></span>
4. <span data-ttu-id="40702-108">Názvy kontejnerů musí mít délku 3 až 63 znaků.</span><span class="sxs-lookup"><span data-stu-id="40702-108">Container names must be from 3 through 63 characters long.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40702-109">Všimněte si, že hello název kontejneru musí být vždycky malá písmena.</span><span class="sxs-lookup"><span data-stu-id="40702-109">Note that hello name of a container must always be lowercase.</span></span> <span data-ttu-id="40702-110">Pokud název kontejneru zahrnout velké písmeno nebo jinak porušíte pravidla pojmenování kontejneru hello, může se zobrazit chyba 400 (Chybný požadavek).</span><span class="sxs-lookup"><span data-stu-id="40702-110">If you include an upper-case letter in a container name, or otherwise violate hello container naming rules, you may receive a 400 error (Bad Request).</span></span> 
> 
> 

