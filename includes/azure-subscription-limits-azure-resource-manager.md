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

<sup>2</sup> To zahrnuje účty služby Storage úrovně Standard i Premium. Pokud potřebujete více než 200 účtů úložiště, vytvořte žádost prostřednictvím [podpory Azure](https://azure.microsoft.com/support/faq/). Tým Azure Storage se na váš obchodní případ podívá a může schválit až 250 účtů úložiště.

<sup>3</sup> Můžete použít neomezený počet značek na předplatné. Počet značek na prostředek nebo skupinu prostředků je omezený na 15. Pokud je počet značek 10 000 nebo méně, Resource Manager vrací pouze [seznam jedinečných názvů a hodnot značek](/rest/api/resources/tags#Tags_List) v předplatném. Pokud však počet překročí 10 000, stále můžete najít prostředek podle značky.  

<sup>4</sup> Tyto funkce se už nevyžadují se skupinami prostředků Azure a Azure Resource Managerem.

> [!NOTE]
> Je důležité zdůraznit, že pro jádra virtuálního počítače platí omezení celkového počtu na oblast i omezení velikosti podle řady (Dv2, F atd.) na oblast a tato omezení se vynucují samostatně.  Představte si například předplatné s omezením celkového počtu 30 jader virtuálního počítače na oblast USA – východ, omezením počtu 30 jader na řadu A a 30 jader na řadu D.  V tomto předplatném by bylo možné nasadit 30 virtuálních počítačů řady A1, 30 virtuálních počítačů řady D1 nebo jejich kombinaci, která nepřekročí celkový počet 30 jader (například 10 virtuálních počítačů řady A1 a 20 virtuálních počítačů řady D1).  
> <!-- -->
> 
> 

