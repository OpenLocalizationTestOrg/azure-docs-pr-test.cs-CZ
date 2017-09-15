<span data-ttu-id="7d86a-101">V systému DNS (Domain Name) slouží k vyhledání věcí na Internetu.</span><span class="sxs-lookup"><span data-stu-id="7d86a-101">The Domain Name System (DNS) is used to locate things on the internet.</span></span> <span data-ttu-id="7d86a-102">Například pokud v prohlížeči zadejte adresu nebo kliknutím na odkaz na webové stránce, používá DNS přeložit domény na IP adresu.</span><span class="sxs-lookup"><span data-stu-id="7d86a-102">For example, when you enter an address in your browser, or click a link on a web page, it uses DNS to translate the domain into an IP address.</span></span> <span data-ttu-id="7d86a-103">IP adresa je použít jako adresu, ale není velmi lidského popisný.</span><span class="sxs-lookup"><span data-stu-id="7d86a-103">The IP address is sort of like a street address, but it's not very human friendly.</span></span> <span data-ttu-id="7d86a-104">Například je mnohem jednodušší pamatovat název DNS jako **contoso.com** než je mějte na paměti, jako je například 192.168.1.88 nebo 2001:0:4137:1f67:24a2:3888:9cce:fea3 IP adresu.</span><span class="sxs-lookup"><span data-stu-id="7d86a-104">For example, it is much easier to remember a DNS name like **contoso.com** than it is to remember an IP address such as 192.168.1.88 or 2001:0:4137:1f67:24a2:3888:9cce:fea3.</span></span>

<span data-ttu-id="7d86a-105">Systém DNS je založen na *záznamy*.</span><span class="sxs-lookup"><span data-stu-id="7d86a-105">The DNS system is based on *records*.</span></span> <span data-ttu-id="7d86a-106">Zaznamenává přidružit konkrétní *název*, jako například **contoso.com**, IP adresu nebo jiný název DNS.</span><span class="sxs-lookup"><span data-stu-id="7d86a-106">Records associate a specific *name*, such as **contoso.com**, with either an IP address or another DNS name.</span></span> <span data-ttu-id="7d86a-107">Při aplikaci, například webový prohlížeč, vyhledává název ve službě DNS, najde záznam a používá, ať odkazuje jako na adresu.</span><span class="sxs-lookup"><span data-stu-id="7d86a-107">When an application, such as a web browser, looks up a name in DNS, it finds the record, and uses whatever it points to as the address.</span></span> <span data-ttu-id="7d86a-108">Pokud je hodnota, které odkazuje na IP adresu, prohlížeč použije tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7d86a-108">If the value it points to is an IP address, the browser will use that value.</span></span> <span data-ttu-id="7d86a-109">Odkazuje na jiný název DNS, aplikace má rozlišení udělat znovu.</span><span class="sxs-lookup"><span data-stu-id="7d86a-109">If it points to another DNS name, then the application has to do resolution again.</span></span> <span data-ttu-id="7d86a-110">Nakonec překlad všechny vyprší za IP adresu.</span><span class="sxs-lookup"><span data-stu-id="7d86a-110">Ultimately, all name resolution will end in an IP address.</span></span>

<span data-ttu-id="7d86a-111">Pokud vytvoříte web Azure, název DNS je automaticky přiřazen k lokalitě.</span><span class="sxs-lookup"><span data-stu-id="7d86a-111">When you create an Azure Website, a DNS name is automatically assigned to the site.</span></span> <span data-ttu-id="7d86a-112">Tento název má formu  **&lt;yoursitename&gt;. azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="7d86a-112">This name takes the form of **&lt;yoursitename&gt;.azurewebsites.net**.</span></span> <span data-ttu-id="7d86a-113">Při přidání webu jako koncový bod Azure Traffic Manager, je pak web přístupné prostřednictvím  **&lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domény.</span><span class="sxs-lookup"><span data-stu-id="7d86a-113">When you add your website as an Azure Traffic Manager endpoint, your website is then accessible through the **&lt;yourtrafficmanagerprofile&gt;.trafficmanager.net** domain.</span></span>

> [!NOTE]
> <span data-ttu-id="7d86a-114">Pokud váš web je nakonfigurovaný jako koncový bod Traffic Manager, použijte **. trafficmanager.net** adres při vytváření záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="7d86a-114">When your website is configured as a Traffic Manager endpoint, you will use the **.trafficmanager.net** address when creating DNS records.</span></span>
> 
> <span data-ttu-id="7d86a-115">Můžete použít pouze záznamy CNAME s nástrojem Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="7d86a-115">You can only use CNAME records with Traffic Manager</span></span>
> 
> 

<span data-ttu-id="7d86a-116">Existují také více typů záznamů, každá má své vlastní funkce a omezení, ale pro weby konfigurované pro jako koncové body Traffic Manager, jsme pouze pro vás k určitému; *CNAME* záznamy.</span><span class="sxs-lookup"><span data-stu-id="7d86a-116">There are also multiple types of records, each with their own functions and limitations, but for websites configured to as Traffic Manager endpoints, we only care about one; *CNAME* records.</span></span>

### <a name="cname-or-alias-record"></a><span data-ttu-id="7d86a-117">Záznam CNAME nebo Alias</span><span class="sxs-lookup"><span data-stu-id="7d86a-117">CNAME or Alias record</span></span>
<span data-ttu-id="7d86a-118">Záznam CNAME se mapuje *konkrétní* název DNS, jako například **mail.contoso.com** nebo **www.contoso.com**, na jiný název domény (kanonický).</span><span class="sxs-lookup"><span data-stu-id="7d86a-118">A CNAME record maps a *specific* DNS name, such as **mail.contoso.com** or **www.contoso.com**, to another (canonical) domain name.</span></span> <span data-ttu-id="7d86a-119">V případě weby Azure s využitím Traffic Manager, je název kanonický domény  **&lt;Moje aplikace >. trafficmanager.net** název domény vašeho profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="7d86a-119">In the case of Azure Websites using Traffic Manager, the canonical domain name is the **&lt;myapp>.trafficmanager.net** domain name of your Traffic Manager profile.</span></span> <span data-ttu-id="7d86a-120">Po vytvoření CNAME tento alias  **&lt;Moje aplikace >. trafficmanager.net** název domény.</span><span class="sxs-lookup"><span data-stu-id="7d86a-120">Once created, the CNAME creates an alias for the **&lt;myapp>.trafficmanager.net** domain name.</span></span> <span data-ttu-id="7d86a-121">Záznam CNAME se přeložit na IP adresu vašeho  **&lt;Moje aplikace >. trafficmanager.net** název domény automaticky, takže pokud se změní IP adresu webu, není nutné provádět žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="7d86a-121">The CNAME entry will resolve to the IP address of your **&lt;myapp>.trafficmanager.net** domain name automatically, so if the IP address of the website changes, you do not have to take any action.</span></span>

<span data-ttu-id="7d86a-122">Jakmile přenos dorazí na Traffic Manager, pak směruje provoz na váš web pomocí metody, který je nakonfigurován pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="7d86a-122">Once traffic arrives at Traffic Manager, it then routes the traffic to your website, using the load balancing method it is configured for.</span></span> <span data-ttu-id="7d86a-123">Toto je pro návštěvníky na svůj web zcela transparentní.</span><span class="sxs-lookup"><span data-stu-id="7d86a-123">This is completely transparent to visitors to your website.</span></span> <span data-ttu-id="7d86a-124">Zobrazí jenom vlastní název domény v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="7d86a-124">They will only see the custom domain name in their browser.</span></span>

> [!NOTE]
> <span data-ttu-id="7d86a-125">Některé domény registrátorů Povolit jenom vám umožní mapovat subdomény, když pomocí záznamu CNAME, jako třeba **www.contoso.com**a nejsou hlavní názvy, například **contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="7d86a-125">Some domain registrars only allow you to map subdomains when using a CNAME record, such as **www.contoso.com**, and not root names, such as **contoso.com**.</span></span> <span data-ttu-id="7d86a-126">Další informace o záznamy CNAME, naleznete v dokumentaci vaším registrátorem <a href="http://en.wikipedia.org/wiki/CNAME_record">položku Wikipedia na záznam CNAME</a>, nebo <a href="http://tools.ietf.org/html/rfc1035">IETF názvy domén – implementace a specifikace</a> dokumentu.</span><span class="sxs-lookup"><span data-stu-id="7d86a-126">For more information on CNAME records, see the documentation provided by your registrar, <a href="http://en.wikipedia.org/wiki/CNAME_record">the Wikipedia entry on CNAME record</a>, or the <a href="http://tools.ietf.org/html/rfc1035">IETF Domain Names - Implementation and Specification</a> document.</span></span>
> 
> 
