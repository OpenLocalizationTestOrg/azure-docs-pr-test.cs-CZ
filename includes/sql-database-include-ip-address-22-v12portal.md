
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, the following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through the new Azure portal
-->


1. <span data-ttu-id="c4516-101">Přihlaste se k [portál Azure](https://portal.azure.com/) v http://portal.azure.com/.</span><span class="sxs-lookup"><span data-stu-id="c4516-101">Log in to the [Azure portal](https://portal.azure.com/) at http://portal.azure.com/.</span></span>
2. <span data-ttu-id="c4516-102">V levém informační zprávě, klikněte na tlačítko **Procházet vše**.</span><span class="sxs-lookup"><span data-stu-id="c4516-102">In the left banner, click **BROWSE ALL**.</span></span> <span data-ttu-id="c4516-103">**Procházet** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="c4516-103">The **Browse** blade is displayed.</span></span>
3. <span data-ttu-id="c4516-104">Přejděte a klikněte na **servery SQL**.</span><span class="sxs-lookup"><span data-stu-id="c4516-104">Scroll and click **SQL servers**.</span></span> <span data-ttu-id="c4516-105">**Servery SQL** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="c4516-105">The **SQL servers** blade is displayed.</span></span>
   
    ![Najít server služby Azure SQL Database na portálu][b21-FindServerInPortal]
4. <span data-ttu-id="c4516-107">Pro usnadnění práce, klikněte na ovládací prvek pro minimalizaci k dřívějšímu **Procházet** okno.</span><span class="sxs-lookup"><span data-stu-id="c4516-107">For convenience, click the minimize control on the earlier **Browse** blade.</span></span>
5. <span data-ttu-id="c4516-108">Do textového pole Filtr začněte zadávat text název vašeho serveru.</span><span class="sxs-lookup"><span data-stu-id="c4516-108">In the filter text box, start typing the name of your server.</span></span> <span data-ttu-id="c4516-109">Řádek, který jste se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c4516-109">Your row is displayed.</span></span>
6. <span data-ttu-id="c4516-110">Klikněte na řádek pro váš server.</span><span class="sxs-lookup"><span data-stu-id="c4516-110">Click the row for your server.</span></span> <span data-ttu-id="c4516-111">Zobrazí se okno pro váš server.</span><span class="sxs-lookup"><span data-stu-id="c4516-111">A blade for your server is displayed.</span></span>
7. <span data-ttu-id="c4516-112">V okně vaší serveru klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="c4516-112">On your server blade, click **Settings**.</span></span> <span data-ttu-id="c4516-113">**Nastavení** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="c4516-113">The **Settings** blade is displayed.</span></span>
8. <span data-ttu-id="c4516-114">Klikněte na tlačítko **brány Firewall**.</span><span class="sxs-lookup"><span data-stu-id="c4516-114">Click **Firewall**.</span></span> <span data-ttu-id="c4516-115">**Nastavení brány Firewall** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="c4516-115">The **Firewall Settings** blade is displayed.</span></span>
   
    ![Klikněte na Nastavení > brány Firewall][b31-SettingsFirewallNavig]
9. <span data-ttu-id="c4516-117">Klikněte na tlačítko **Přidání klienta IP**.</span><span class="sxs-lookup"><span data-stu-id="c4516-117">Click **Add Client IP**.</span></span> <span data-ttu-id="c4516-118">Zadejte název nové pravidlo do textového pole první.</span><span class="sxs-lookup"><span data-stu-id="c4516-118">Type in a name for your new rule into the first text box.</span></span>
10. <span data-ttu-id="c4516-119">Zadejte v vysoké a nízké hodnoty adres IP pro oblast, kterou chcete povolit.</span><span class="sxs-lookup"><span data-stu-id="c4516-119">Type in the low and high IP address values for the range you want to enable.</span></span>
    
    * <span data-ttu-id="c4516-120">Může být užitečný tak, aby měl nízkou hodnotu končit **.0** a vysoce s **.255**.</span><span class="sxs-lookup"><span data-stu-id="c4516-120">It can be handy to have the low value end with **.0** and the high with **.255**.</span></span>
    
    ![Přidat rozsah IP adres umožňuje][b41-AddRange]
11. <span data-ttu-id="c4516-122">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c4516-122">Click **Save**.</span></span>

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
