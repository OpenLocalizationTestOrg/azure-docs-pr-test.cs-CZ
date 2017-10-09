# <a name="securing-docker-containers-in-azure-container-service"></a>Zabezpečení kontejnerů Dockeru ve službě Azure Container Service

Tento článek uvádí důležité informace a doporučení pro zabezpečení kontejnerů Dockeru nasazených ve službě Azure Container Service. Obecně platí mnohé z těchto aspektů tooDocker kontejnery nasazené v Azure nebo jiných prostředích. 

## <a name="image-security"></a>Zabezpečení image

Kontejnery se vytváří z imagí uložených v jednom nebo několika úložištích. Tyto úložiště můžou patřit toopublic nebo privátní kontejneru registrech. Příkladem veřejného registru je [Docker Hub](https://hub.docker.com/). Příkladem privátní registru je hello [Docker důvěryhodné registru](https://docs.docker.com/datacenter/dtr/2.0/), může být nainstalována na místní nebo virtuální privátní cloud. Existují také cloudové služby privátních registrů kontejnerů, mezi které patří služba [Azure Container Registry](../articles/container-registry/container-registry-intro.md).

### <a name="public-and-private-images"></a>Veřejné a privátní image
Stejně jako u jakéhokoli veřejně publikovaného softwarového balíčku obecně platí, že veřejně dostupné image kontejneru nezaručují zabezpečení. Image kontejnerů se skládají z několika vrstev softwaru a každá z nich může obsahovat ohrožení zabezpečení. Je klíče toounderstand hello počátek obrázku hello kontejneru, včetně hello vlastníka hello bitové kopie (Pokud není spolehlivý zdroj, nebo není toodetermine), hello softwaru vrstvy, které se skládá z a hello verze softwaru. 

Například, pokud přejdete toohello oficiální [nginx úložiště](https://hub.docker.com/_/nginx/) Docker hub a přejděte toohello **značky** kartě uvidíte barevně slabá místa zabezpečení v každé bitové kopie. Každá barva znázorňuje hello ohrožení zabezpečení vrstvy softwaru hello bitové kopie. Další informace o zjišťování ohrožení zabezpečení v Docker Hubu najdete v článku [Understanding official repos on Docker Hub](https://blog.docker.com/2015/06/understanding-official-repos-docker-hub/) (Principy oficiálních úložišť v Docker Hubu).

![Image nginx v Docker Hubu](./media/container-service-security/docker-hub-nginx.png)

Podniky jsou pro vás úzce související se zabezpečením a tooprotect sami před útokem zabezpečení by měl ukládání a načítání bitové kopie z privátní registru, například Azure kontejneru registru nebo Docker důvěryhodné registru. V přidání tooproviding spravované privátní registru, registru kontejner Azure podporuje [ověřování na základě hlavní služby](../articles/container-registry/container-registry-authentication.md) prostřednictvím Azure Active Directory pro toky základní ověřování, včetně přístupu na základě rolí pro jen pro čtení, zápisu a oprávnění vlastníka.

### <a name="image-security-scanning"></a>Kontrola zabezpečení imagí

I když se používá privátní registru, je toouse bitovou kopii vhodné řešení pro ověření další bezpečnostní kontrolu. Jednotlivé úrovně softwaru do bitové kopie kontejneru je potenciálně náchylné k chybám toovulnerabilities nezávisle na jiných vrstev v bitové kopii hello kontejneru. Jako stále společnosti začít s nasazením jejich úlohy v produkčním prostředí založené na technologiích kontejneru, stane se skenování obrázků důležité tooensure prevence bezpečnostní hrozby před jejich organizace. 

Zabezpečení sledování a kontrolu řešení, jako [Twistlock](https://www.twistlock.com/2016/11/07/twistlock-supports-azure-container-registry) a [akvamarínová zabezpečení](http://blog.aquasec.com/image-vulnerability-scanning-in-azure-container-registry), mimo jiné, lze použít tooscan kontejneru obrázků v privátní registru a identifikovala potenciální ohrožení zabezpečení. Je důležité poskytnout toounderstand hello hloubku kontrolu této hello různá řešení. Některá řešení například můžou u vrstev image křížově ověřovat jenom výskyt známých ohrožení zabezpečení. Tato řešení nemusí být možné tooverify vrstva s obrázkem softwaru vytvořené pomocí určitého softwaru správce balíčku. Jiná řešení mají hlubší integraci kontroly a můžou najít ohrožení zabezpečení v jakémkoli zabaleném softwaru.

### <a name="production-deployment-rules-and-audit"></a>Pravidla a audit produkčního nasazení
Jakmile je aplikace nasazená v produkčním prostředí, je nezbytné tooset několik tooensure pravidla, která Image použít v produkčním prostředí jsou zabezpečené a obsahovat žádné ohrožení zabezpečení.

* Platí pravidlo bitové kopie s chybami, i dílčí by nemělo toorun v produkčním prostředí. Kromě toho všechny bitové kopie nasazení v produkčním prostředí by v ideálním případě ho uložit ve s výběrem přístupné tooa privátní registru několika. Je také důležité tookeep hello počet produkční bitové kopie malých tooensure, aby mohly být spravovány efektivně.

* Vzhledem k tomu, že je pevný toopinpoint hello původu softwaru z bitové kopie veřejně dostupné kontejneru, je vhodné toobuild obrázkům ze zdroje tooensure znalostí hello původu hello vrstvy. Při povrchy ohrožení zabezpečení v bitové kopii samoobslužné integrovaný kontejneru, nalézt zákazníkům rychlejší řešení tooa cesta. Veřejné Image, zákazníci potřebovat toofind hello kořenovém veřejné image toofix ji nebo získat jiné zabezpečené image od vydavatele hello.

* Důkladně naskenovaného obrázku nasazen v provozním prostředí není zaručena toobe až toodate pro hello životního cyklu aplikace hello. Chyby zabezpečení může hlášené pro vrstvy hello bitové kopie, které nebyly dříve známé nebo byly zavedeny po hello produkčním nasazení. Pravidelné auditování bitové kopie nasazení v produkčním prostředí je nutné tooidentify bitové kopie, které jsou zastaralá nebo nebyly aktualizovány za chvíli. Jeden může použít blue zelená nasazení metod a vrácení upgradu mechanismy tooupdate kontejneru obrázky bez výpadků. Skenování obrázků lze provádět pomocí nástrojů popsaných v předcházející části hello. 

* Kanál průběžnou integraci (CI) toobuild obrázky a kontrolu integrované zabezpečení může pomoct udržovat zabezpečené privátní registrech s obrázky zabezpečeného kontejneru. Hello integrovaná řešení CI hello zjišťování ohrožení zabezpečení zajistí, že jsou bitové kopie, které úspěšně projít všemi testy hello nabídnutých toohello privátní registru z výroby, které jsou nasazeny úlohy. Selhání kanálu CI zajistí, že citlivé obrázky nejsou vložena do privátní registru hello používá pro nasazení v produkčním prostředí zatížení. V případě velkého počtu imagí kanál také automatizuje kontrolu jejich zabezpečení. Jinak může být ruční auditování imagí za účelem zjišťování ohrožení zabezpečení zdlouhavé a náchylné k chybám.

## <a name="host-level-container-isolation"></a>Izolace kontejneru na úrovni hostitele
Když zákazník nasazuje aplikace typu kontejner do prostředků Azure, nasazují se na úrovni předplatného ve skupinách prostředků a nejsou určené pro více tenantů. To znamená, že pokud zákazník sdílí předplatné s ostatními, nejsou žádné hranice, které mohou být vytvořeny mezi dvěma nasazeními v hello stejné předplatné. Z tohoto důvodu není zaručeno zabezpečení na úrovni kontejneru. 

Je také důležité toounderstand, že kontejnery sdílet hello jádra a prostředky hello hello hostitele, (což je v Azure Container Service je virtuální počítač Azure v clusteru). Proto je nutné kontejnery v produkčním prostředí spouštět v uživatelském režimu bez oprávnění správce. Spuštěny kontejner s oprávněními kořenové může ohrozit hello celé prostředí. S přístupem na úrovni kořenového adresáře v kontejneru může se hacker získat přístup oprávnění kořenového toohello na hostiteli hello. Kromě toho je důležité toorun kontejnery s systémy souborů jen pro čtení. Tím se zabrání někdo, kdo má přístup toohello dojde k ohrožení kontejneru toowrite škodlivých skriptů toohello systém souborů a získání přístupu tooother soubory. Podobné je to důležité toolimit hello prostředky (například paměť, procesor a šířku pásma sítě) přidělené tooa kontejneru. To pomáhá zabránit hackerům v zabraly pro sebe celou prostředky a provést neplatný aktivity, například podvod platební karty nebo bitcoin dolování, což může bránit ve spouštění na hello hostiteli nebo clusteru jiných kontejnerů.

## <a name="orchestrator-considerations"></a>Důležité informace o orchestrátorech

Pro každý orchestrátor dostupný ve službě Azure Container Service platí specifické aspekty zabezpečení. Například byste měli omezit přímé SSH přístup tooorchestrator uzlech v kontejneru služby. Místo toho byste měli používat každý orchestrator uživatelského rozhraní nebo nástroje příkazového řádku (například `kubectl` pro Kubernetes) toomanage hello kontejneru prostředí bez přístup k hostitelům hello. Další informace najdete v tématu [zkontrolujte cluster Kubernetes, DC/OS nebo Docker Swarm vzdáleného připojení tooa](../articles/container-service/kubernetes/container-service-connect.md).

Další bezpečnostní orchestrator specifické informace najdete v tématu hello následující prostředky:

* **Kubernetes:**[Security Best Practices for Kubernetes Deployment](http://blog.kubernetes.io/2016/08/security-best-practices-kubernetes-deployment.html) (Osvědčené postupy zabezpečení pro nasazení Kubernetes)

* **DC/OS:**[Securing Your Cluster](https://dcos.io/docs/1.8/administration/securing-your-cluster/) (Zabezpečení clusteru)

* **Docker Swarm:**[Docker Security](https://www.docker.com/docker-security) (Zabezpečení Dockeru)

## <a name="next-steps"></a>Další kroky

* Další informace o zabezpečení architektura a kontejner Docker najdete v tématu [Úvod tooContainer zabezpečení](https://www.docker.com/sites/default/files/WP_IntrotoContainerSecurity_08.19.2016.pdf).

* Informace o zabezpečení platformy Azure najdete v tématu hello [Azure Security Center](https://www.microsoft.com/en-us/trustcenter/cloudservices/azure).
