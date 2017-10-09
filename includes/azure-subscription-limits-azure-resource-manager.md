| Prostředek | Výchozí omezení | Maximální omezení |
| --- | --- | --- |
| Počet virtuálních počítačů na [předplatné](../articles/billing-buy-sign-up-azure-subscription.md) |10 000<sup>1</sup> na oblast |10 000 na oblast |
| Celkový počet jader virtuálního počítače na [předplatné](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> na oblast | Kontaktování podpory |
| Počet jader virtuálního počítače podle řady (Dv2, F atd.) na [předplatné](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> na oblast | Kontaktování podpory |
| Počet [spolusprávců](../articles/billing-add-change-azure-subscription-administrator.md) na předplatné |Unlimited |Unlimited |
| [Účty služby Storage](../articles/storage/common/storage-create-storage-account.md) na předplatné |200 |200<sup>2</sup> |
| [Skupiny prostředků](../articles/azure-resource-manager/resource-group-overview.md) na předplatné |800 |800 |
| [Skupiny dostupnosti](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) na předplatné |2 000 na oblast |2 000 na oblast |
| Čtení rozhraní API Resource Manageru |15 000 za hodinu |15 000 za hodinu |
| Zápisy rozhraní API Resource Manageru |1 200 za hodinu |1 200 za hodinu |
| Velikost požadavku rozhraní API Resource Manageru |4 194 304 bajtů |4 194 304 bajtů |
| Počet značek na předplatné<sup>3</sup> |Bez omezení |Bez omezení |
| Jedinečné výpočty značek na předplatné<sup>3</sup> | 10 000 | 10 000 |
| [Cloudové služby](../articles/cloud-services/cloud-services-choose-me.md) na předplatné |Neuvedeno<sup>4</sup> |Neuvedeno<sup>4</sup> |
| [Skupiny vztahů](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) na předplatné |Neuvedeno<sup>4</sup> |Neuvedeno<sup>4</sup> |

<sup>1</sup> Výchozí omezení se liší podle typu kategorie nabídky, jako je bezplatná zkušební verze nebo průběžné platby, a řadě, jako je Dv2, F, G atd.

<sup>2</sup> To zahrnuje účty služby Storage úrovně Standard i Premium. Pokud potřebujete více než 200 účtů úložiště, vytvořte žádost prostřednictvím [podpory Azure](https://azure.microsoft.com/support/faq/). Hello týmu Azure Storage bude zkontrolovat váš případ obchodní a může schválit too250 úložiště účtů.

<sup>3</sup> Můžete použít neomezený počet značek na předplatné. Hello počet značky pro každý prostředek nebo skupina prostředků je omezené too15. Správce prostředků pouze vrátí [seznam jedinečný název značky a hodnoty](/rest/api/resources/tags#Tags_List) v předplatném hello při hello počet značek je 10 000 nebo méně. Však můžete stále najít prostředek podle značky při hello počet překračuje 10 000.  

<sup>4</sup>tyto funkce se už nevyžadují s skupiny prostředků Azure a hello Azure Resource Manager.

> [!NOTE]
> Je důležité tooemphasize s jader virtuálního počítače místní celkový limit, jakož i místní za omezení velikosti řady (Dv2, F atd.), který vynutí se samostatně.  Představte si například předplatné s omezením celkového počtu 30 jader virtuálního počítače na oblast USA – východ, omezením počtu 30 jader na řadu A a 30 jader na řadu D.  Toto předplatné bude mít možnost A1 toodeploy 30 virtuálních počítačů nebo 30 virtuálních počítačů D1 nebo kombinaci hello dva tooexceed celkem 30 jader (například 10 virtuálních počítačů A1 a 20 D1 virtuálních počítačů).  
> <!-- -->
> 
> 

