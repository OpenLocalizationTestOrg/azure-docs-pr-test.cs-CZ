---
title: "nasazení PaaS aaaSecuring | Microsoft Docs"
description: " Rady pro pochopení hello zabezpečení výhody PaaS a jinými modely cloudové služby a další doporučené postupy pro zabezpečení vašeho nasazení Azure PaaS. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: techlake
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: terrylan
ms.openlocfilehash: b94fe03b6f3a43d82e1e6e834f10a423e4c1db95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-deployments"></a>Zabezpečení nasazení PaaS

Tento článek obsahuje informace, které vám pomůže:

- Pochopit výhody zabezpečení hello hostování aplikací v cloudu hello
- Vyhodnocení hello zabezpečení výhod platforma jako služba (PaaS) a ostatní modely cloudové služby
- Změnit váš výběr zabezpečení přístupu zaměřené na síti tooan hraniční zaměřená na identitu zabezpečení na
- Implementovat obecné PaaS zabezpečení podle doporučení

## <a name="cloud-security-advantages"></a>Výhody zabezpečení cloudu
V cloudu hello nejsou toobeing výhody zabezpečení. V prostředí místní organizace pravděpodobně mít unmet odpovědnosti a omezené prostředky k dispozici tooinvest v zabezpečení, který vytvoří prostředí, kde se útočníci možné tooexploit ohrožení zabezpečení na všechny vrstvy.

![Výhody zabezpečení letopočtu cloudu][1]

Organizace jsou možné tooimprove jejich detekce hrozeb a časy odezvy pomocí funkce zabezpečení založené na cloudu a cloudu intelligence zprostředkovatele.  Přepínáním poskytovatele cloudové toohello odpovědnosti organizace můžete získat další pokrytí zabezpečení, což jim umožní tooreallocate zabezpečení prostředků a nároky tooother obchodní priority.

## <a name="division-of-responsibility"></a>Dělení zodpovědnosti
Je důležité toounderstand hello dělení zodpovědnosti mezi vámi a společností Microsoft. Místní, vlastní hello celý zásobník, ale když přesouváte toohello cloudu některé odpovědnosti přenést tooMicrosoft. Hello následující odpovědnost matice zobrazí hello oblasti hello zásobníku v SaaS, IaaS a PaaS nasazení, která je zodpovědná za a Microsoft je zodpovědná za.

![Odpovědnost zóny][2]

Pro všechny typy nasazení cloudu které vlastníte dat a identity. Jste odpovědni za ochranu hello zabezpečení dat a identity, místních prostředků a hello cloudu součásti, které řídíte (který se liší podle typu služby).

Odpovědnosti, které jsou vždy zachovány vy, bez ohledu na to hello typ nasazení, jsou:

- Data
- Koncové body
- Účet
- Správa přístupu

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>Výhody zabezpečení PaaS cloudu modelu služby
Pomocí umožňuje hello stejné matice odpovědnost, podívejte se na výhody zabezpečení hello nasazení Azure PaaS a místní.

![Výhody zabezpečení PaaS][3]

Počínaje hello dolní části hello zásobník hello fyzické infrastruktuře, Microsoft snižuje běžné rizik a odpovědnosti. Protože hello cloudu Microsoftu je průběžně monitorovat společností Microsoft, je pevný tooattack. Nemá smysl pro útočník toopursue hello cloudu Microsoftu jako cíl. Pokud útočník hello obsahuje mnoho peněz a prostředky, útočník hello je pravděpodobně toomove na tooanother cíli.  

Uprostřed hello hello zásobníku není žádný rozdíl mezi nasazení PaaS a místně. V hello aplikační vrstvu a vrstva správy hello účet a přístup mají podobné rizika. V hello další kroky části tohoto článku provedeme vás toobest postupy pro odstranění nebo minimalizace těchto rizik.

Hello horní části zásobníku hello, řízení dat a rights management můžete provést pro jeden riziko, že může být omezeny správy klíčů. (Správa klíčů je popsaná v osvědčených postupů). I správu klíčů je další odpovědnost, máte oblastí v nasazení PaaS, můžete přejít tookey řízení zdrojů již nebude mít toomanage.

Hello platformy Azure také poskytuje silnou ochranu DDoS pomocí různých technologií založené na síti. Všechny typy metody ochrany založené na síti DDoS však mít jejich omezení na základě na propojení a za datacenter. toohelp vyhnout hello dopad útoků DDoS velké, můžete využít výhod funkce cloudu Azure základní povolení jste tooquickly a automaticky škálování toodefend proti útokům DDoS. Budeme věnovat podrobněji na tom, jak to můžete provést v hello doporučené postupy články.

## <a name="modernizing-hello-defenders-mindset"></a>Modernizace hello defender přistupovat
Nasazení PaaS má posunutí ve vaší celkové toosecurity přístup. Posunutí z nutnosti toocontrol všechno sami toosharing odpovědnost se společností Microsoft.

Další důležité rozdíl mezi PaaS a tradičních místních nasazení, je nové zobrazení co definuje hello primární zabezpečení hraniční. V minulosti hello primární místní zabezpečení hraniční síti byla a nejvíce místní zabezpečení návrhů použití hello síti jako jeho pivot primární zabezpečení. U nasazení PaaS jsou lépe obsloužených zvažování identity toobe hello primární zabezpečení hraniční.

## <a name="identity-as-hello-primary-security-perimeter"></a>Identity jako primární zabezpečení hraniční hello
Jeden z pět základních vlastností hello technologie cloud computing je široký přístup k síti, takže je zaměřená na síti myslím méně důležité. Hello cílem mnohem technologie cloud computing je uživatelé tooallow tooaccess prostředky bez ohledu na umístění. Pro většinu uživatelů jejich umístění se děje toobe někde hello Internetu.

Hello následující obrázek ukazuje, jak byla vyvinuta hello zabezpečení hraniční ze hraniční sítě hraniční tooan identity. Zabezpečení se změní na menší o nutné chránit vaši síť a o nutné chránit vaše data, jakož i správa hello zabezpečení uživatelů a aplikací. Hello klíčovým rozdílem je, že chcete toopush zabezpečení blíže toowhat je důležité tooyour společnosti.

![Identity jako nový hraniční zabezpečení][4]

Na začátku služby Azure PaaS (například webové role a Azure SQL) k dispozici žádné nebo téměř žádné tradiční sítě hraniční obrany. Má se za to, že účel hello elementu byl toohello toobe vystavené Internetu (webové role) a poskytuje nové hraniční hello (například objektů BLOB nebo Azure SQL), ověření.

Postupy moderní zabezpečení Předpokládejme, že nežádoucí osoba hello porušila hello hraniční sítě. Proto moderní obrany postupy přesunutí tooidentity. Organizace musí vytvořit zabezpečení na základě identity hraniční s silné ověřování a autorizace kontrolu (osvědčených postupů).

## <a name="recommendations-for-managing-hello-identity-perimeter"></a>Doporučení pro správu identity hraniční hello

Principy a vzory pro hraniční síť hello byly k dispozici pro dekád. Naproti tomu hello odvětví má relativně. méně zkušenosti s používáním identity jako primární zabezpečení hraniční hello. S třídou uvedená jsme shromáždily dostatek zkušeností tooprovide některé obecná doporučení, které jsou ověřené v poli hello a použít všechny služby PaaS tooalmost.

Následující Hello shrnuje obecné toomanaging nejlepší přístup postupy hraniční vaší identity.

- **Neztraťte klíče nebo přihlašovací údaje** zabezpečení klíčů a přihlašovacích údajů je nezbytné toosecure nasazení PaaS. Došlo ke ztrátě klíče a přihlašovací údaje jsou běžné potíže. Jeden dobrým řešením je toouse centralizovaného řešení, kde klíče a tajné klíče mohou být uložené v modulech hardwarového zabezpečení (HSM). Azure poskytuje modul hardwarového zabezpečení v cloudu hello s [Azure Key Vault](../key-vault/key-vault-whatis.md).
- **Nevkládejte do zdrojového kódu nebo Githubu přihlašovací údaje a jiné tajné** hello jen nejhorší věcí než došlo ke ztrátě klíče a přihlašovací údaje má neoprávněná osoba získat přístup k toothem. Útočníci se může tootake výhod robota technologie toofind klíče a tajné klíče uložené v kódu úložiště, jako je například Githubu. Neumísťujte klíče a tajné klíče v těchto veřejných úložišť zdrojového kódu.
- **Chránit vaše rozhraní pro správu virtuálních počítačů v hybridním službám PaaS a IaaS** IaaS a PaaS služby spouštět na virtuálních počítačích (VM). V závislosti na typu hello služby, jsou k dispozici několik rozhraní pro správu, umožňují tooremote přímo spravovat tyto virtuální počítače. Vzdálená správa protokoly, jako [protokolu Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell), [protokol RDP (Remote Desktop)](https://support.microsoft.com/kb/186607), a [vzdáleného prostředí PowerShell](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) lze použít. Obecně doporučujeme nepovolujte tooVMs přímé vzdáleného přístupu z Internetu hello. Pokud je k dispozici, měli byste použít alternativních přístupech například pomocí virtuální privátní síť do virtuální sítě Azure. Pokud nejsou k dispozici alternativní přístupy a pak ověřte, že používáte komplexní přístupová hesla a pokud je k dispozici, dvojúrovňového ověřování (například [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)).
- **Silné ověřování a autorizace platformy**

  - Použijte federované identity ve službě Azure AD místo úložiště vlastní uživatele. Při použití federovaných identit, můžete využít přístupu na základě platformy a delegovat správu hello partnerů tooyour oprávnění identity. Přístup federovaných identit je obzvláště důležité ve scénářích, když zaměstnanci budou ukončeny a že informace potřebuje toobe projeví prostřednictvím více identit a autorizace systémy.
  - Použití platformy zadat mechanismy ověřování a autorizace místo vlastní kód. Hello důvodem je, že vývoj vlastní ověřovací kód může být chyba náchylné k chybám. Většina vaší vývojáři nejsou odborníky na zabezpečení a jsou pravděpodobně toobe vědět hello odlišnosti a hello nejnovější vývoj ve ověřování a autorizace. Komerční kód (například ze společnosti Microsoft) je často hojně zabezpečení zkontrolovat.
  - Používání služby Multi-Factor authentication. Službu Multi-Factor authentication je hello aktuální standardní pro ověřování a autorizaci, protože při ní nedochází nedostatky v zabezpečení hello vyplývajících z uživatelské jméno a heslo typy ověřování. Rozhraní pro správu Azure (portál vzdáleného prostředí PowerShell) hello tooboth přístup a toocustomer čelí služby by se měly navrhovat a nakonfigurovány toouse [Azure Multi-Factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md).
  - Použijte standardní ověřovací protokoly, jako je například OAuth2 a protokolu Kerberos. Tyto protokoly byly hojně sdílené zkontrolovány a pravděpodobně jsou implementované jako součást vaší knihovny platformy pro ověřování a autorizaci.

## <a name="next-steps"></a>Další kroky
V tomto článku jsme zaměřuje na výhody zabezpečení nasazení Azure PaaS. Dále se naučíte doporučené postupy pro zabezpečení vašich PaaS webové a mobilní řešení. Začneme s Azure App Service, Azure SQL Database a Azure SQL Data Warehouse. Jakmile články na doporučené postupy pro jinými službami Azure k dispozici, bude k dispozici odkazy v následující seznamu hello:

- [Azure App Service](security-paas-applications-using-app-services.md)
- [Databáze Azure SQL a Azure SQL Data Warehouse](security-paas-applications-using-sql.md)
- Azure Storage
- Mezipaměť REDIS systému Azure
- Azure Service Bus
- Brány firewall webových aplikací

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png
