Cloudová řešení Azure jsou založená na virtuálních počítačích (emulaci hardwaru fyzických počítačů), které umožňují agilní balení softwarových nasazení a lepší konsolidaci prostředků než fyzický hardware. [Docker](https://www.docker.com) kontejnery a hello docker ekosystém má výrazně rozšířené hello způsoby, kterými můžete vyvíjet, odeslání a spravovat distribuované softwaru. Kód aplikace v kontejneru je izolovaná od hello hostitele virtuálních počítačů a jiných kontejnerů v hello stejného virtuálního počítače. Tato izolace poskytuje flexibilnější možnosti vývoje a nasazení.

Azure nabízí následující hodnoty Docker hello:

* [Mnoho](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) [různých](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) způsoby toocreate Docker hostuje pro kontejnery toosuit vaší situaci
* Hello [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) vytvoří clustery hostitelů kontejneru pomocí orchestrators, jako třeba **marathon** a **swarm**.
* [Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md) a [šablony skupiny prostředků](../articles/resource-group-authoring-templates.md) toosimplify nasazení a aktualizace komplexních distribuovaných aplikací
* Integrace se širokou škálou vlastních nebo i opensourcových nástrojů pro správu konfigurace.

A protože prostřednictvím kódu programu můžete vytvořit virtuální počítače a Linux kontejnerů v Azure, můžete také použít virtuální počítač a kontejner *orchestration* nástroje toocreate skupiny virtuálních počítačů (VM) a toodeploy aplikace uvnitř i Linux kontejnery a teď [Windows kontejnery](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

Tento článek popisuje nejen tyto koncepty na vysoké úrovni, obsahuje taky tuny odkazy toomore informací, kurzy, a produkty související využití toocontainer a cluster v Azure. Pokud vědět, to vše a chcete jen hello odkazy, jsou přímo tady v [nástroje pro práci s kontejnery](#tools-for-working-with-azure-vms-and-containers).

## <a name="hello-difference-between-virtual-machines-and-containers"></a>Hello rozdíl mezi virtuálními počítači a kontejnery
Virtuální počítače běží uvnitř izolovaného prostředí virtualizace hardwaru, které poskytuje [hypervisor](http://en.wikipedia.org/wiki/Hypervisor). V Azure, hello [virtuální počítače](https://azure.microsoft.com/services/virtual-machines/) služba zpracovává všechno, co pro vás: vytváření virtuálních počítačů pomocí volba hello operačního systému a konfigurace &mdash;nebo tím, že nahrajete vlastní image virtuálního počítače. Virtuální počítače jsou technologie prověřené, "boj Posílená", a neexistují celou řadu nástrojů k dispozici toomanage hello operačního systému a aplikací obsahují.  Aplikace ve virtuálním počítači jsou skryta hello hostitelský operační systém. Hello virtuálních počítačů z hello hlediska aplikace nebo uživatele na virtuálním počítači, se zobrazí toobe autonomní fyzického počítače.

[Kontejnery Linux](http://en.wikipedia.org/wiki/LXC) a ty vytvořené a hostované pomocí nástroje docker, nepoužívejte izolaci tooprovide hypervisoru. S kontejnery hostitel kontejneru hello používá proces a funkce izolace systému souboru hello Linux jádra tooexpose toohello kontejneru, jeho aplikace, některé funkce jádra a svůj vlastní systém izolované souboru. Z hello hlediska aplikace běžících v rámci kontejneru hello kontejner se zobrazí toobe jedinečné instance operačního systému. Aplikace v kontejneru nevidí žádné procesy ani jiné prostředky vně svého kontejneru.

V kontejneru Dockeru se využívá mnohem méně prostředků než ve virtuálním počítači. Kontejnery docker použít izolace a provádění model aplikací, které nesdílí hello jádra hello Docker hostitele. Hello kontejneru je mnohem nižší nároky disk jako neobsahuje hello celý operačního systému. Čas spuštění je podstatně kratší a požadované místo na disku je výrazně menší než v případě virtuálního počítače.
Kontejnery Windows hello stejné výhody zajistit jako kontejnery Linux pro aplikace, které běží v systému Windows. Kontejnery Windows podporují formát obrázku Docker hello a Docker rozhraní API, ale může být také spravovat pomocí prostředí PowerShell. Pro kontejnery Windows jsou dostupné dva kontejnerové moduly runtime, Windows Server Containers a Hyper-V Containers. Modul Hyper-V Containers poskytuje další úroveň izolace, protože každý kontejner hostuje v superoptimalizovaném virtuálním počítači. informace o používání Windows kontejnery najdete toolearn [o kontejnery Windows](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview). tooget práce s kontejnery Windows v Azure, zjistěte, jak příliš[nasazení clusteru Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).

## <a name="what-are-containers-good-for"></a>K čemu jsou kontejnery dobré?

Kontejnery můžou zlepšit:

* kód aplikace Hello rychlost lze vyvinuté a sdílet široce
* rychlost Hello a přitom jistotu, že aplikace může být testována.
* rychlost Hello a přitom jistotu, že aplikace se dá nasadit.

Kontejnery běží na hostiteli, kterým je operační systém, a v Azure, to znamená virtuální počítač Azure. I když už rádi hello nápad kontejnerů, stále budete tooneed infrastrukturu virtuálních počítačů hostování hello kontejnery, ale hello výhody, kontejnery nezáleží na které virtuálního počítače používají (i když jestli hello kontejnerem chce, aby Linux nebo Windows prostředí pro spuštění bude důležitá, například).


## <a name="what-are-containers-good-for"></a>K čemu jsou kontejnery dobré?
Jsou vhodné pro celou řadu věcí, ale budou podporovat&mdash;stejně jako [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) a [Azure Service Fabric](../articles/service-fabric/service-fabric-overview.md)&mdash;hello vytvoření jedné služby, orientované mikroslužbu distribuované aplikace v aplikaci, pro kterou design je založen na další malé, bez možnosti složení částí ne na větší, více důrazně párované součásti.

To platí hlavně ve veřejných cloudových prostředích, jako je Azure, ve kterých si virtuální počítače pronajímáte, kdy a kde chcete. Nejenom že získáte izolaci, rychlý výboj a nástroje pro orchestraci, ale můžete taky efektivněji rozhodovat a infrastruktuře aplikací.

Představte si třeba, že máte nasazení, které se skládá z 9 virtuálních počítačů Azure s velkou velikostí pro distribuovanou aplikaci s vysokou dostupností. Pokud hello součástí této aplikace mohou být nasazeny v kontejnerech, může být schopný toouse jenom 4 virtuální počítače a nasazení součásti vaší aplikace uvnitř 20 kontejnery pro redundanci a vyrovnávání zatížení.

Toto je jenom jako příklad samozřejmě, ale pokud to provedete ve vašem scénáři, můžete upravit toousage špičky víc kontejnerů než další virtuální počítače Azure a použít hello zbývající celkové vytížení procesoru mnohem efektivnější než dřív.

Kromě toho existuje mnoho scénářů, které nemohou být poskytovány tooa mikroslužeb přístup; budete vědět, co nejlépe zda mikroslužeb a kontejnerů, vám pomůže.

### <a name="container-benefits-for-developers"></a>Výhody kontejnerů pro vývojáře
Obecně platí je snadné toosee technologie kontejneru je krok dál, ale existují i další specifické výhody. Podívejme se na příklad hello Docker kontejnerů. Toto téma nebude podrobně hluboko Docker nyní (čtení [co je Docker?](https://www.docker.com/whatisdocker/) pro tento scénář, nebo [wikipedia](http://wikipedia.org/wiki/Docker_%28software%29)), ale Docker a jeho ekosystém nabízejí strávíte výhody tooboth vývojáře a IT Odborníci v oblasti.

Vývojáři trvat tooDocker kontejnery rychle, protože nad všemi umožňuje použití snadno Linux a Windows kontejnerů:

* Jejich můžete použít jednoduchý, přírůstkové příkazy toocreate pevné bitovou kopii, která je snadno toodeploy a můžete automatizovat vytváření těchto bitových kopií pomocí soubor docker
* Tyto bitové kopie pomocí snadno jednoduchý, můžou sdílet [git](https://git-scm.com/)– styl nabízené a načítat příkazy příliš[veřejné](https://registry.hub.docker.com/) nebo [registrech privátní docker](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Můžou se zaměřit na izolované aplikační komponenty, ne na počítače.
* Můžou využívat širokou škálu nástrojů, které zvládají kontejnery Docker a různé základní image.

### <a name="container-benefits-for-operations-and-it-professionals"></a>Výhody kontejnerů pro provozní pracovníky a odborníky na IT
IT a odborníci v oblasti operations také těžit z kombinace hello kontejnery a virtuálních počítačů.

* Služby v kontejneru jsou izolované od hostitelského spouštěcího prostředí virtuálního počítače.
* Kód v kontejneru je ověřitelně identický.
* Služby v kontejneru je možné spouštět, zastavovat a rychle přesouvat mezi vývojovým, testovacím a produkčním prostředím.

Funkce, jako jsou tyto&mdash;a neexistují další&mdash;buzení zavedených firmy, kde informace pro odborníky v oblasti technologie organizace mají úlohy hello přizpůsobování prostředků&mdash;včetně čistý výpočetní výkon&mdash; požadované toonot toohello úlohy pouze zůstat v podnikání, ale zvýšit spokojenost zákazníků a dosáhnout. Malé firmy, nezávislí dodavatelé softwaru a startupy mají hello přesně stejnou požadavek, ale může popisují ho jinak.

## <a name="what-are-virtual-machines-good-for"></a>K čemu jsou dobré virtuální počítače?
Virtuální počítače zadejte hello páteřní cloud computing a který nemění. Pokud virtuální počítače spustit pomaleji, mají větší nároky disku a přímo nemapovaly tooa mikroslužeb architektura, mají velmi důležité výhody:

1. Standardně poskytují mnohem větší výchozí ochranu zabezpečení pro hostitelský počítač.
2. Podporují všechny hlavní operační systémy a konfigurace aplikací.
3. Dlouhodobě mají ekosystémy nástrojů pro příkazy a ovládání.
4. Poskytují hello spuštění prostředí toohost kontejnery

poslední položky Hello je důležité, protože obsahují aplikace stále vyžaduje konkrétní operační systém a typ procesoru, v závislosti na hello volání, které bude aplikace hello. Je důležité tooremember nainstalovat kontejnery na virtuálních počítačích, protože obsahují aplikace hello chcete toodeploy; kontejnery nejsou nahrazení pro virtuální počítače nebo operační systémy.

## <a name="high-level-feature-comparison-of-vms-and-containers"></a>Porovnání základních funkcí virtuálních počítačů a kontejnerů
Hello následující tabulka popisuje velmi vysoké úrovni hello druh funkce rozdíly, který&mdash;bez mnohem další práci&mdash;uzavřeli kontejnery virtuálních počítačů a Linux. Upozorňujeme, že některé funkce žádoucí, může být vyšší nebo nižší v závislosti na své vlastní aplikace potřebuje a že stejně jako u veškerého softwaru práce navíc poskytuje vyšší funkce podpory, zejména v oblasti hello zabezpečení.

| Funkce | Virtuální počítače | Kontejnery |
|:--- | --- | --- |
| „Výchozí“ podpora zabezpečení |tooa vyšší úroveň |tooa mírně menší míře |
| Vyžadovaná paměť na disku |Kompletní operační systém plus aplikace |Jenom požadavky aplikací |
| Doba trvání toostart |Podstatně delší: Spuštění operačního systému plus načítání aplikací |Podstatně kratší: jen aplikace potřebují toostart, protože je již spuštěn jádra |
| Přenositelnost |Přenositelné po příslušné přípravě |Přenositelné v rámci formátu image; obvykle menší |
| Automatizace imagí |Výrazně se liší v závislosti na operačním systému a aplikacích |[Registr Dockeru](https://registry.hub.docker.com/), další |

## <a name="creating-and-managing-groups-of-vms-and-containers"></a>Vytváření a správa skupin virtuálních počítačů a kontejnerů
V této chvíli si pravděpodobně každý architekt, vývojář nebo specialista na provoz IT myslí: „Tohle všechno dokážu automatizovat sám. Není to nic jiného než datové centrum jako služba!“

Jste správné, může být a existují libovolný počet systémů, řada z nich může už používáte, které můžete buď spravovat skupiny virtuálních počítačů Azure a vložit vlastní kód pomocí skriptů, často s hello [CustomScriptingExtension pro systém Windows](https://msdn.microsoft.com/library/azure/dn781373.aspx) nebo Hello [CustomScriptingExtension pro Linux](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/). Můžete automatizovat nasazení Azure pomocí PowerShellu nebo skriptů rozhraní příkazového řádku Azure (a už jste to pravděpodobně i udělali).

Tyto možnosti jsou často pak jako migrované tootools [Puppet](https://puppetlabs.com/) a [Chef](https://www.chef.io/) tooautomate hello vytvoření a konfigurace pro virtuální počítače ve velkém měřítku. (Tady jsou některé odkazy příliš[pomocí těchto nástrojů Azure](#tools-for-working-with-containers).)

### <a name="azure-resource-group-templates"></a>Šablony skupin prostředků Azure
Více nedávno Azure vydané hello [správu prostředků Azure](../articles/resource-manager-deployment-model.md) rozhraní API REST a aktualizované prostředí PowerShell a rozhraní příkazového řádku Azure nástroje toouse ho snadno. Můžete nasadit, upravit nebo znovu nasadit topologie celá aplikace pomocí [šablon Azure Resource Manageru](../articles/resource-group-authoring-templates.md) s použitím rozhraní API pro správu prostředků Azure hello:

* Hello [portál Azure pomocí šablony](https://github.com/Azure/azure-quickstart-templates)&mdash;pomocného parametru, použijte tlačítko "DeployToAzure" hello
* Hello [rozhraní příkazového řádku Azure](../articles/virtual-machines/linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="deployment-and-management-of-entire-groups-of-azure-vms-and-containers"></a>Nasazení a správa celých skupin kontejnerů a virtuálních počítačů Azure
Existuje několik oblíbených systémů, které umožňují nasadit celé skupiny virtuálních počítačů a nainstalovat na ně Docker (nebo jiné hostitelské systémy kontejnerů Linux) jako na automatizovatelnou skupinu. Přímé odkazy, najdete v části hello [kontejnery a nástroje](#containers-and-vm-technologies) části níže. Existuje několik systémů, které provádějí tento tooa většího nebo menšího rozsahu, a tento seznam není vyčerpávající. V závislosti na vašich dovednostech a scénářích pro vás můžou nebo nemusejí být užitečné.

Docker má vlastní sadu nástrojů pro vytváření virtuálních počítačů ([docker-machine](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) a taky nástroj pro správu clusteru s vyrovnáváním zatížení ([swarm](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Kromě toho hello [rozšíření virtuálního počítače Azure Docker](https://github.com/Azure/azure-docker-extension/blob/master/README.md) se dodává s výchozí podpora [ `docker-compose` ](https://docs.docker.com/compose/), které můžete nasadit kontejnery aplikací nakonfigurovat napříč několika kontejnerů.

Můžete taky vyzkoušet [systém DCOS (Data Center Operating System) od Mesosphere](http://docs.mesosphere.com). Orchestrátoru DC/OS je založena na hello open-source [mesos](http://mesos.apache.org/) "distribuovaných systémů jádra", která umožňuje vám tootreat vašem datovém centru jako jedna adresovatelné služba. DCOS nabízí integrované balíčky pro několik důležitých systémů, jako je [Spark](http://spark.apache.org/) a [Kafka](http://kafka.apache.org/) (a další), a taky integrované služby, jako je [Marathon](https://mesosphere.github.io/marathon/) (systém pro řízení kontejnerů) a [Chronos](https://mesos.github.io/chronos/) (distribuovaný plánovač). Mesos vychází z poznatků zjištěných Twitterem, AirBnb a dalšími webovými společnostmi. Můžete také použít **swarm** jako jádro Orchestrace hello.

[Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/) je opensourcový systém pro správu skupin kontejnerů a virtuálních počítačů, který vychází z poznatků a zkušeností Googlu. Můžete použít i [kubernetes s weave síťovou podporu tooprovide](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave).

[Deis](http://deis.io/overview/) je otevřenou zdroje "Platformy jako služba" (PaaS), díky které je snadno toodeploy a spravovat aplikace na serverech. Deis sestavení při Docker a CoreOS tooprovide lightweight PaaS s inspirovaných Heroku pracovního postupu.

[CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html), linuxová distribuce s optimalizovanými nároky na místo, podporou Dockeru a vlastním kontejnerovým systémem [rkt](https://github.com/coreos/rkt), taky nabízí nástroj pro správu skupin kontejnerů s názvem [fleet](https://coreos.com/fleet/docs/latest/).

Ubuntu, další velice oblíbená distribuce Linuxu, zajišťuje dobrou podporu Dockeru a podporuje taky [linuxové clustery (typu LXC)](https://help.ubuntu.com/lts/serverguide/lxc.html).

## <a name="tools-for-working-with-azure-vms-and-containers"></a>Nástroje pro práci s kontejnery a virtuálními počítači Azure
Práce s kontejnery a virtuálními počítači Azure vyžaduje nástroje. Tato část obsahuje seznam jenom některé z hello nejvíce užitečné nebo důležité koncepty a nástroje pro o kontejnerů, skupin a hello větší konfiguraci a orchestraci nástroje používat s nimi.

> [!NOTE]
> Tato oblast se mění amazingly rychle a při naše nejlepší tookeep uděláme Toto téma a jeho propojení se toodate, může být dobře možné úloh. Ujistěte se, že hledání zajímavé témata tookeep až toodate!
>
>

### <a name="containers-and-vm-technologies"></a>Technologie virtuálních počítačů a kontejnerů
Některé technologie kontejnerů Linux:

* [Docker](https://www.docker.com)
* [LXC](https://linuxcontainers.org/)
* [CoreOS a rkt](https://github.com/coreos/rkt)
* [Open Container Project](http://opencontainers.org/)
* [RancherOS](http://rancher.com/rancher-os/)

Odkazy pro kontejnery Windows:

* [Kontejnery Windows](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)

Odkazy na Docker pro Visual Studio:

* [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/core/docker/visual-studio-tools-for-docker)

Nástroje Dockeru:

* [Démon Dockeru](https://docs.docker.com/installation/#installation)
* Klienti Docker
  * [Klient Docker pro Windows v Chocolatey](https://chocolatey.org/packages/docker)
  * [Pokyny k instalaci Dockeru](https://docs.docker.com/installation/#installation)

Docker v Microsoft Azure:

* [Rozšíření Docker VM pro Linux v Azure](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Uživatelská příručka k rozšíření Azure Docker VM](https://github.com/Azure/azure-docker-extension/blob/master/README.md)
* [Pomocí hello Docker rozšíření virtuálního počítače z hello rozhraní příkazového řádku Azure (Azure CLI)](../articles/virtual-machines/linux/classic/cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Pomocí hello Docker rozšíření virtuálního počítače z hello portálu Azure](../articles/virtual-machines/linux/classic/portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Jak toouse docker počítače v Azure](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Jak toouse docker s swarm v Azure](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Začínáme s prostředím Docker a Compose v Azure](../articles/virtual-machines/linux/docker-compose-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Rychle pomocí šablony toocreate skupiny prostředků Azure hostitelů Docker v Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)
* [Hello integrovanou podporu pro `compose` ](https://github.com/Azure/azure-docker-extension#11-public-configuration-keys) pro obsaženým aplikacím
* [Implementace privátního registru Docker v Azure](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Linuxové distribuce a příklady Azure:

* [CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html)

Konfigurace, správa clusterů a orchestrace kontejnerů:

* [Fleet v systému CoreOS](https://coreos.com/fleet/docs/latest/)
* Deis

  * [Dokončení Průvodce tooautomated Kubernetes nasazení clusteru s CoreOS a vlákna](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
  * [Vizualizátor Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Mesos](http://mesos.apache.org/)

  * [DCOS (Data Center Operating System) od Mesosphere](https://docs.mesosphere.com/1.7/overview/design/azure-container-service/)
* [Jenkins](https://jenkins.io/) a [Hudson](http://hudson-ci.org/)

  * [Modul plug-in Jenkins agenta virtuálního počítače pro Azure](https://wiki.jenkins.io/display/JENKINS/Azure+VM+Agents+plugin)
  * [Úložiště GitHub: Plug-In Jenkins Storage pro Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)
  * [Jiný výrobce: Plug-in Hudson Slave pro Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
  * [Jiný výrobce: Plug-in Hudson Storage pro Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Azure Automation](https://azure.microsoft.com/services/automation/)

  * [Video: Jak tooUse Azure Automation se virtuální počítače s Linuxem](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* Powershell DSC pro Linux

  * [Blog: Jak toodo Powershell DSC pro Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
  * [GitHub: DockerClientDSC](https://github.com/anweiss/DockerClientDSC)

## <a name="next-steps"></a>Další kroky
Podívejte se na témata [Docker](https://www.docker.com) a [Kontejnery Windows](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

<!--Anchors-->
[microservices]: http://martinfowler.com/articles/microservices.html
[microservice]: http://martinfowler.com/articles/microservices.html
<!--Image references-->
