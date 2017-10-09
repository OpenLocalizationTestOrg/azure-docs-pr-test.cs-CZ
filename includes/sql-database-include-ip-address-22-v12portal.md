
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, hello following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through hello new Azure portal
-->


1. <span data-ttu-id="3d821-101">Přihlaste se toohello [portál Azure](https://portal.azure.com/) v http://portal.azure.com/.</span><span class="sxs-lookup"><span data-stu-id="3d821-101">Log in toohello [Azure portal](https://portal.azure.com/) at http://portal.azure.com/.</span></span>
2. <span data-ttu-id="3d821-102">V levém banner hello, klikněte na **Procházet vše**.</span><span class="sxs-lookup"><span data-stu-id="3d821-102">In hello left banner, click **BROWSE ALL**.</span></span> <span data-ttu-id="3d821-103">Hello **Procházet** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="3d821-103">hello **Browse** blade is displayed.</span></span>
3. <span data-ttu-id="3d821-104">Přejděte a klikněte na **servery SQL**.</span><span class="sxs-lookup"><span data-stu-id="3d821-104">Scroll and click **SQL servers**.</span></span> <span data-ttu-id="3d821-105">Hello **servery SQL** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="3d821-105">hello **SQL servers** blade is displayed.</span></span>
   
    ![V aplikaci portál hello najít server služby Azure SQL Database][b21-FindServerInPortal]
4. <span data-ttu-id="3d821-107">Pro usnadnění práce, klikněte na tlačítko hello minimalizovat ovládací prvek v dříve hello **Procházet** okno.</span><span class="sxs-lookup"><span data-stu-id="3d821-107">For convenience, click hello minimize control on hello earlier **Browse** blade.</span></span>
5. <span data-ttu-id="3d821-108">Do textového pole hello filtr začněte zadávat text hello název vašeho serveru.</span><span class="sxs-lookup"><span data-stu-id="3d821-108">In hello filter text box, start typing hello name of your server.</span></span> <span data-ttu-id="3d821-109">Řádek, který jste se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="3d821-109">Your row is displayed.</span></span>
6. <span data-ttu-id="3d821-110">Klikněte na řádek hello pro váš server.</span><span class="sxs-lookup"><span data-stu-id="3d821-110">Click hello row for your server.</span></span> <span data-ttu-id="3d821-111">Zobrazí se okno pro váš server.</span><span class="sxs-lookup"><span data-stu-id="3d821-111">A blade for your server is displayed.</span></span>
7. <span data-ttu-id="3d821-112">V okně vaší serveru klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="3d821-112">On your server blade, click **Settings**.</span></span> <span data-ttu-id="3d821-113">Hello **nastavení** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="3d821-113">hello **Settings** blade is displayed.</span></span>
8. <span data-ttu-id="3d821-114">Klikněte na tlačítko **brány Firewall**.</span><span class="sxs-lookup"><span data-stu-id="3d821-114">Click **Firewall**.</span></span> <span data-ttu-id="3d821-115">Hello **nastavení brány Firewall** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="3d821-115">hello **Firewall Settings** blade is displayed.</span></span>
   
    ![Klikněte na Nastavení > brány Firewall][b31-SettingsFirewallNavig]
9. <span data-ttu-id="3d821-117">Klikněte na tlačítko **Přidání klienta IP**.</span><span class="sxs-lookup"><span data-stu-id="3d821-117">Click **Add Client IP**.</span></span> <span data-ttu-id="3d821-118">Zadejte název pro nové pravidlo do textového pole pro první hello.</span><span class="sxs-lookup"><span data-stu-id="3d821-118">Type in a name for your new rule into hello first text box.</span></span>
10. <span data-ttu-id="3d821-119">Typ v hello nízkou a vysokou IP adres hodnoty pro rozsah hello chcete tooenable.</span><span class="sxs-lookup"><span data-stu-id="3d821-119">Type in hello low and high IP address values for hello range you want tooenable.</span></span>
    
    * <span data-ttu-id="3d821-120">Může být užitečný toohave hello nízkou hodnotu končit **.0** a hello vysoké s **.255**.</span><span class="sxs-lookup"><span data-stu-id="3d821-120">It can be handy toohave hello low value end with **.0** and hello high with **.255**.</span></span>
    
    ![Přidat tooallow rozsah adres IP][b41-AddRange]
11. <span data-ttu-id="3d821-122">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3d821-122">Click **Save**.</span></span>

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
