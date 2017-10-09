## <a name="scenario"></a>Scénář
Tento dokument provede nasazení, které používá několik síťových adaptérů ve virtuálních počítačích v určité scénáře. V tomto scénáři máte Dvojúrovňová IaaS zatížení hostované v Azure. Každý vrstva je nasazená ve vlastní podsíti ve virtuální síti (VNet). úroveň front-endu Hello se skládá z několika webových serverů, seskupeny ve službě Vyrovnávání zatížení nastavena pro zajištění vysoké dostupnosti. úroveň back-end Hello se skládá z několika databázových serverů. Tyto databázové servery, nasadí se dvěma síťovými adaptéry, každý, jeden pro přístup k databázi, hello dalších pro správu. Hello scénář zahrnuje také toocontrol skupiny zabezpečení sítě (Nsg), jaký provoz je povolený tooeach podsítě a síťový adaptér v nasazení hello. Hello obrázek níže ukazuje základní architekturu hello tohoto scénáře.  

![Scénář MultiNIC](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

