---
title: "Průvodce zabezpečením úložiště aaaAzure | Microsoft Docs"
description: "Podrobnosti o hello mnoho metody zabezpečení Azure Storage, včetně, ale bez omezení tooRBAC, šifrování služby úložiště, šifrování na straně klienta, protokolu SMB 3.0 a Azure Disk Encryption."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 6f931d94-ef5a-44c6-b1d9-8a3c9c327fb2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: d406ff0d6b45c6107d0276ad9e65c331078ce792
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-security-guide"></a>Průvodce zabezpečením služby Azure Storage
## <a name="overview"></a>Přehled
Úložiště Azure poskytuje komplexní sadu funkcí zabezpečení, které společně umožňují vývojářům toobuild zabezpečených aplikací. účet úložiště Hello, samotné lze zabezpečit pomocí řízení přístupu na základě Role a Azure Active Directory. Data byla zabezpečená během přenosu mezi aplikací a Azure pomocí [šifrování na straně klienta](storage-client-side-encryption.md), HTTPS nebo SMB 3.0. Data lze nastavit toobe šifrují automaticky při zápisu tooAzure úložiště pomocí [šifrování služby úložiště (SSE)](storage-service-encryption.md). Operačního systému a datové disky používat virtuální počítače lze nastavit toobe šifrovat pomocí [Azure Disk Encryption](../security/azure-security-disk-encryption.md). Delegovaný přístup toohello datových objektů ve službě Azure Storage můžete udělit pomocí [sdílené přístupové podpisy](storage-dotnet-shared-access-signature-part-1.md).

Tento článek přináší přehled každé z těchto funkcí zabezpečení, které lze použít s Azure Storage. Odkazy jsou k dispozici tooarticles, který získáte podrobnosti o jednotlivých funkcí, můžete snadno provést další šetření na každého tématu.

Zde jsou toobe hello témata popsaná v tomto článku:

* [Zabezpečení roviny Management](#management-plane-security) – zabezpečení vašeho účtu úložiště

  Hello správu roviny se skládá z hello prostředky využívané toomanage účtu úložiště. V této části se budeme mluvit o hello modelu nasazení Azure Resource Manager a přístupu účty úložiště tooyour toocontrol toouse řízení přístupu na základě Role (RBAC). Bude také faktorech mluvíme o správě klíče účtu úložiště a jak tooregenerate je.
* [Zabezpečení dat roviny](#data-plane-security) – zabezpečení přístupu tooYour dat

  V této části se podíváme povolením přístupu toohello skutečné datové objekty v účtu úložiště, jako jsou objekty BLOB, soubory, fronty a tabulky, pomocí uložené zásady přístupu a podpisy sdíleného přístupu. Vám nabídneme SAS úrovně služby a SAS účtu úrovni. Také ukážeme, jak toolimit přistupovat k tooa konkrétní IP adresu (nebo rozsah IP adres), jak použít protokol hello toolimit tooHTTPS a jak toorevoke a sdíleného přístupového podpisu bez čekání na jeho tooexpire.
* [Šifrování během přenosu](#encryption-in-transit)

  Tato část pojednává o tom, jak toosecure dat při přenosu do nebo z Azure Storage. Budeme mluvit o hello doporučuje použití protokolu HTTPS a hello šifrování používá protokol SMB 3.0 pro sdílené složky Azure File. Také jsme bude trvat podívejte se na straně klienta šifrování, které vám umožní tooencrypt hello data předtím, než bude převedena do úložiště v aplikaci klienta a toodecrypt hello data, jakmile se přenáší z úložiště.
* [Šifrování v klidovém stavu](#encryption-at-rest)

  Bude faktorech mluvíme o šifrování služby úložiště (SSE) a jak můžete ji povolit pro účet úložiště, což vede k objektů BLOB bloku, objekty BLOB stránky a doplňovací objekty BLOB se šifrují automaticky, když zapsána tooAzure úložiště. Podíváme se také na tom, jak můžete použít Azure Disk Encryption a prozkoumat základní rozdíly hello a případů šifrování disku versus SSE versus šifrování na straně klienta. Podíváme se stručně na soulad se standardem FIPS pro USA Počítače, Government.
* Pomocí [Storage Analytics](#storage-analytics) tooaudit přístupu Azure Storage

  Tato část popisuje, jak toofind informace v hello úložiště analýzy protokolů pro žádost. Jsme budete si prohlédněte analytika úložiště skutečná data protokolu a v tématu Jak toodiscern zda požadavku s hello úložiště účet klíč s sdílený přístupový podpis, nebo anonymně a jestli byla úspěšná nebo neúspěšná.
* [Povolení založené na prohlížeči klientů pomocí CORS](#Cross-Origin-Resource-Sharing-CORS)

  Tato část pojednává o tom, tooallow prostředků mezi zdroji (CORS) pro sdílení. Budeme mluvit o přístup do jiných domén, a jak toohandle se s možnostmi CORS hello integrována do úložiště Azure.

## <a name="management-plane-security"></a>Zabezpečení roviny Management
Hello správu roviny se skládá z operace, které ovlivňují hello účet úložiště, sám sebe. Můžete například vytvořit nebo odstranit účet úložiště, získáte seznam účtů úložiště v předplatném, načtení klíčů účtu úložiště hello nebo obnovit klíče účtu úložiště hello.

Když vytvoříte nový účet úložiště, můžete vybrat model nasazení Classic nebo Resource Manager. Hello Klasický model vytváření prostředků v Azure jenom umožňuje vše nebo nic přístup toohello předplatného a pak hello účet úložiště.

Tato příručka se zaměřuje na hello modelu Resource Manager, který je hello doporučené možnosti pro vytváření účtů úložiště. S účty úložiště hello Resource Manager, a nikoli uvádějící přístup toohello celé předplatné můžete řídit přístup na více konečné rovinu správy úrovně toohello pomocí řízení přístupu na základě Role (RBAC).

### <a name="how-toosecure-your-storage-account-with-role-based-access-control-rbac"></a>Jak toosecure úložiště účet pomocí řízení přístupu na základě rolí (RBAC)
Budeme mluvit o novinky RBAC a jak můžete použít. Každé předplatné Azure zahrnuje službu (adresář) Azure Active Directory. Uživatelé, skupiny a aplikací z adresáře můžete udělit přístup k prostředkům toomanage v hello předplatné Azure, které používají model nasazení Resource Manager hello. Toto je odkazované tooas řízení přístupu na základě Role (RBAC). toomanage tento přístup, můžete použít hello [portál Azure](https://portal.azure.com/), hello [nástrojů příkazového řádku Azure](../cli-install-nodejs.md), [prostředí PowerShell](/powershell/azureps-cmdlets-docs), nebo hello [API REST poskytovatele prostředků úložiště Azure ](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Pomocí modelu Resource Manager hello uveďte účet úložiště hello v prostředku skupiny a řízení přístupu toohello správu rovině tohoto účtu konkrétní úložiště pomocí služby Azure Active Directory. Například můžete uživatelům konkrétní hello možnost tooaccess hello klíče účtu úložiště, můžete zobrazit informace o účtu úložiště hello jiných uživatelů, ale nemůže přístupové klíče účtu úložiště hello.

#### <a name="granting-access"></a>Udělení přístupu
Přístup uděluje přiřazením toousers hello odpovídající RBAC role, skupiny a aplikace, v pravém oboru hello. toogrant přístup toohello celé předplatné, přiřadíte roli na úrovni předplatného hello. Můžete udělit přístup tooall hello prostředků ve skupině prostředků tak, že udělíte oprávnění toohello prostředků skupinu jako celek. Můžete také přiřadit konkrétní role toospecific prostředky, například účty úložiště.

Zde jsou hlavní body hello, je nutné, aby tooknow o používání RBAC tooaccess hello management operace účet úložiště Azure:

* Při přiřazení přístupu v podstatě přiřadit roli toohello účtu, který má přístup toohave. Můžete řídit přístup toohello operace používá toomanage daný účet úložiště, ale ne toohello data objekty v účtu hello. Například můžete udělit oprávnění tooretrieve hello vlastnosti účtu úložiště hello (například redundance), ale není tooa kontejneru nebo dat v rámci kontejneru uvnitř úložiště objektů Blob.
* Pro někdo toohave oprávnění tooaccess hello datové objekty v účtu úložiště hello jim můžete poskytnout tooread oprávnění, hello klíče účtu úložiště a tento uživatel pak můžete tyto klíče tooaccess hello objekty BLOB, fronty, tabulky a soubory.
* Role jde přiřadit tooa konkrétní uživatelský účet, skupiny uživatelů nebo tooa konkrétní aplikaci.
* Každá role má seznam akcí a není akce. Například role Přispěvatel virtuálních počítačů hello má akce "listKeys", který umožňuje číst toobe klíče účtu úložiště hello. Hello Přispěvatel má "Není akce" jako aktualizace hello přístup pro uživatele v hello služby Active Directory.
* Role pro úložiště zahrnují (ale nejsou omezeny) hello následující:

  * Vlastník – můžou spravovat všechno včetně přístupu.
  * Přispěvatel – mohou provádět nic hello vlastníka můžete kromě přiřadit přístup. Někdo s touto rolí můžete zobrazit a obnovit klíče účtu úložiště hello. S hello klíče účtu úložiště získají přístup k hello datových objektů.
  * Čtečka – mohou prohlížet informace o účtu úložiště hello, s výjimkou tajných klíčů. Například pokud přiřazení role s oprávněními pro čtečky na toosomeone účet úložiště hello mohou prohlížet vlastnosti hello hello účtu úložiště, ale nemohou žádné změny vlastnosti toohello nebo zobrazit hello klíče účtu úložiště.
  * Přispěvatel účet úložiště – můžou spravovat účet úložiště hello – může číst hello předplatného skupiny prostředků a prostředků a vytvářet a spravovat předplatné nasazení skupiny prostředků. Můžete také přístup k klíče účtu úložiště hello, které pak znamená, že získají přístup k datové roviny hello.
  * Správce přístupu uživatelů – můžou spravovat uživatelský účet úložiště toohello přístup. Například se můžete udělit čtečky přístup tooa konkrétního uživatele.
  * Virtuální počítač Přispěvatel – můžou spravovat virtuální počítače, ale není hello úložiště účet toowhich, které jsou připojené. Tato role může seznam hello klíče účtu úložiště, což znamená, že hello toowhom uživatele přiřaďte tuto roli můžete aktualizovat roviny data hello.

    Aby mohl uživatel toocreate virtuální počítač mají toobe možné toocreate hello odpovídající soubor VHD v účtu úložiště. toodo, že potřebují toobe možné tooretrieve hello úložiště účet klíč a předejte ji toohello API vytváření hello virtuálních počítačů. Proto musí mít toto oprávnění, může být uveden hello klíče účtu úložiště.
* Hello možnost toodefine vlastní role je funkce, která vám umožní toocompose sadu akcí ze seznamu dostupných akcí, které lze provést na prostředky Azure.
* Hello uživatel má toobe nastavit v Azure Active Directory, než bude možné přiřadit role toothem.
* Můžete vytvořit sestavu kdo udělit nebo odvolat jaký typ přístupu do a z kterého a v jaké oboru pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure hello.

#### <a name="resources"></a>Zdroje
* [Řízení přístupu na základě role v Azure Active Directory](../active-directory/role-based-access-control-configure.md)

  Tento článek vysvětluje hello řízení přístupu na základě Role v Azure Active Directory a jak to funguje.
* [RBAC: vestavěné role](../active-directory/role-based-access-built-in-roles.md)

  Tento článek podrobně všechny dostupné v RBAC hello předdefinované role.
* [Principy nasazení podle modelu Resource Manager a klasického nasazení](../azure-resource-manager/resource-manager-deployment-model.md)

  Tento článek vysvětluje hello nasazení Resource Manager a modely nasazení classic a vysvětluje hello výhody použití Resource Manager a prostředek skupiny hello. Vysvětluje, jak fungují v rámci modelu Resource Manager hello hello výpočetní Azure, sítě a poskytovatelé úložiště.
* [Správa řízení přístupu na základě rolí pomocí rozhraní REST API hello](../active-directory/role-based-access-control-manage-access-rest.md)

  Tento článek ukazuje, jak toouse hello REST API toomanage RBAC.
* [Referenci rozhraní API REST poskytovatele prostředků úložiště Azure](https://msdn.microsoft.com/library/azure/mt163683.aspx)

  Toto je odkaz hello pro hello rozhraní API můžete použít toomanage účtu úložiště prostřednictvím kódu programu.
* [Tooauth Příručka pro vývojáře s rozhraním API pro Azure Resource Manager](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

  Tento článek ukazuje, jak pomocí tooauthenticate hello rozhraní API Resource Manager.
* [Řízení přístupu na základě role pro Microsoft Azure z Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

  Toto je odkaz tooa, který je na webu Channel 9 video z konference Ignite 2015 MS hello. V této relaci se mluvit o přístup k správy a možnosti vytváření sestav v Azure a zkoumat osvědčených postupů zabezpečení přístupu tooAzure odběry pomocí služby Azure Active Directory.

### <a name="managing-your-storage-account-keys"></a>Správa klíčů k účtu úložiště
Účet úložiště, že klíče jsou 512 bitů řetězce vytvořené Azure, které společně s hello úložiště název, účtu může být uložený v účtu storage hello, např. objekty BLOB, entit v rámci tabulky, fronty zpráv a soubory ve sdílené složce Azure File použít tooaccess hello datových objektů. Řízení přístupu toohello úložiště účet klíče řídí přístup toohello roviny data pro tento účet úložiště.

Každý účet úložiště má dva klíče, označuje tooas "Klíč 1" a "Klíčem 2" v hello [portál Azure](http://portal.azure.com/) a v hello rutiny prostředí PowerShell. To je možné znovu generovat ručně pomocí několika metod, včetně, ale bez omezení toousing hello [portál Azure](https://portal.azure.com/), prostředí PowerShell, hello rozhraní příkazového řádku Azure nebo programově pomocí hello Klientská knihovna pro .NET úložiště nebo hello Azure REST API služeb úložiště.

Existují libovolný počet důvodů tooregenerate klíče účtu úložiště.

* V pravidelných intervalech z bezpečnostních důvodů může vygenerovat je.
* Klíče účtu úložiště by obnovit, pokud někdo spravované toohack do aplikace a načíst klíč hello, pevně zakódované nebo uložený v konfiguračním souboru, bude mít účet úložiště tooyour úplný přístup.
* Jiné případ opětovné generování klíče je Pokud váš tým používá aplikaci Průzkumník úložišť, která uchovává hello klíč účtu úložiště a některého z členů týmu hello opustí. aplikace Hello by dále toowork, poskytnete jim přístup k účtu úložiště tooyour, jakmile jsou pryč. Toto je ve skutečnosti hello hlavním důvodem, které se vytvářely úrovni účtu sdílené přístupové podpisy – můžete úrovni účtu SAS místo ukládání hello přístupové klíče v konfiguračním souboru.

#### <a name="key-regeneration-plan"></a>Plán obnovení klíče
Nechcete, aby toojust znovu generovat hello klíče, který používáte bez některé plánování. Pokud tak učiníte, může oříznutí všechny přístup toothat účet úložiště, což může způsobit přerušení hlavní. Z tohoto důvodu existují dva klíče. Vždy znovu generujte jeden klíč najednou.

Předtím, než můžete znovu vygenerovat klíče, ujistěte se, že máte seznam všechny aplikace, které jsou závislé na účtu úložiště hello, jakož i jiné služby, který používáte v Azure. Například pokud používáte službu Azure Media Services, které jsou závislé na vašem účtu úložiště, musíte znovu synchronizovat hello přístupové klíče s touto službou po opětovném vygenerování klíče hello. Pokud používáte jakékoli aplikace, jako je například Průzkumníka úložiště, budete potřebovat tooprovide hello nové klíče toothose aplikace také. Všimněte si, že pokud máte virtuální počítače, jejichž soubory VHD jsou uloženy v účtu úložiště hello, budou se nevztahuje opakované generování klíčů účtu úložiště hello.

Můžete znovu vygenerovat klíče v hello portálu Azure. Jakmile budou klíče obnovovány může trvat až too10 minut toobe synchronizována v rámci služby úložiště.

Až budete připraveni, tady je obecný postup hello s podrobnostmi o tom, jak byste měli změnit váš klíč. V takovém případě předpokládá hello je, že používáte klíč 1 a budete toochange všechno, co toouse klíče 2 místo.

1. Znovu vygenerujte klíč 2 tooensure, že se jedná o zabezpečené. To provedete v hello portálu Azure.
2. Ve všech aplikací hello se uloží klíč úložiště hello změňte hello úložiště klíčů toouse klíč 2 je novou hodnotu. Testování a publikování aplikace hello.
3. Po všech hello aplikace a služby jsou a spuštěna úspěšně, znovu vygenerovat klíč 1. To zajistí, že kdokoliv toowhom výslovně jste nedali hello nový klíč, nebudou mít přístup k účtu úložiště toohello.

Pokud aktuálně používáte klíč 2, můžete použít stejný proces, ale názvy klíčů zpětné hello hello.

Můžete migrovat přes několik dní, změna každý nový klíč aplikace toouse hello a jeho publikování. Poté, co se dokončí všechny z nich, by měl poté přejděte zpět a obnovit původní klíč hello, přestane fungovat.

Další možností je klíč účtu úložiště tooput hello v [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) jako tajný klíč a mít klíč hello načtení aplikací z ní. Když znovu vygenerovat klíč hello a aktualizovat hello Azure Key Vault, potom aplikace hello nepotřebují toobe znovu nasazena, protože jejich vyzvedne, až bude hello nový klíč z hello Azure Key Vault automaticky. Všimněte si, že nemáte aplikace hello čtení klíče hello pokaždé, když to potřebujete, nebo můžete mezipaměti v paměti a pokud se nezdaří, při použití, hello klíč znovu načíst ze hello Azure Key Vault.

Použití Azure Key Vault, přidá se také další úroveň zabezpečení pro vaše klíče úložiště. Pokud použijete tuto metodu, budete mít klíče pevně zakódované úložiště hello nikdy v konfigurační soubor, který odebere tento způsob někdo získávání klíčů toohello bez konkrétní oprávnění přístupu.

Další výhodou použití Azure Key Vault je, že můžete také ovládat přístup tooyour klíče pomocí Azure Active Directory. To znamená, že můžete udělit přístup toohello několik aplikací, které potřebují tooretrieve hello klíče z Azure Key Vault a vědět, že jiné aplikace nebudou mít tooaccess hello klíče bez toho, abyste je konkrétně oprávnění.

Poznámka: doporučujeme toouse jen jeden z hello klíče ve všech aplikacích v hello stejný čas. Pokud používáte klíč 1 některé místech a 2 klíč k ostatním, které není být schopný toorotate klíče bez některé aplikace ztráty přístupu.

#### <a name="resources"></a>Zdroje
* [Informace o účtech úložiště Azure](storage-create-storage-account.md#regenerate-storage-access-keys)

  Tento článek nabízí přehled účty úložiště a popisuje zobrazení, kopírování a opětovné generování přístupových klíčů k úložišti.
* [Referenci rozhraní API REST poskytovatele prostředků úložiště Azure](https://msdn.microsoft.com/library/mt163683.aspx)

  Tento článek obsahuje odkazy toospecific článků o načítání klíče účtu úložiště hello a opakované generování klíčů k účtu úložiště hello k účtu Azure pomocí hello REST API. Poznámka: Toto je pro účty úložiště Resource Manager.
* [Operace na účty úložiště](https://msdn.microsoft.com/library/ee460790.aspx)

  Tento článek v hello referenční dokumentace rozhraní API REST úložiště Service Manager obsahuje články toospecific odkazy na načítání a opakované generování klíčů účtu úložiště hello pomocí hello REST API. Poznámka: Toto je pro účty úložiště Classic hello.
* [Můžete dát sbohem všem tookey správu – spravovat přístup tooAzure úložiště dat pomocí služby Azure AD](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

  Tento článek ukazuje, jak toouse služby Active Directory toocontrol přístupové klíče tooyour Azure Storage v Azure Key Vault. Také ukazuje, jak toouse Azure Automation úlohy tooregenerate hello klíče hodinu.

## <a name="data-plane-security"></a>Zabezpečení dat roviny
Zabezpečení dat roviny odkazuje toohello metody použité toosecure hello datové objekty uložené ve službě Azure Storage – hello objekty BLOB, fronty, tabulky a soubory. Jsme viděli při přenosu dat hello metody tooencrypt hello dat a zabezpečení, ale jak tedy můžete o povolení přístupu toohello objekty?

V podstatě existují dvě metody pro řízení přístupu toohello datových objektů, sami. Hello, nejprve je řízení přístupu toohello klíčů k účtu úložiště a hello druhý používá sdílené přístupové podpisy toogrant přístup toospecific datových objektů pro konkrétní množství času.

Jeden toonote výjimka je veřejný přístup tooyour objektů BLOB můžete povolit nastavením hello úroveň přístupu pro hello kontejner, který obsahuje objekty BLOB hello odpovídajícím způsobem. Pokud jste nastavili přístup pro kontejner tooBlob nebo kontejner, vám umožní veřejný přístup pro čtení pro objekty BLOB hello v tomto kontejneru. To znamená, že každý, kdo má adresu URL ukazující tooa objektů blob v tomto kontejneru ho otevřete v prohlížeči bez pomocí sdíleného přístupového podpisu nebo s hello klíče účtu úložiště.

### <a name="storage-account-keys"></a>Klíče účtu úložiště
Klíče účtu úložiště jsou 512 bitů řetězce vytvořené Azure, který spolu s názvem účtu úložiště hello může být uložený v účtu storage hello použité tooaccess hello datových objektů.

Můžete například čtení objektů BLOB, zápis tooqueues, vytváření tabulek a upravovat soubory. Mnoho z těchto akcí lze provést pomocí hello Azure portálu nebo pomocí jedné z mnoha aplikací Průzkumníka úložiště. Můžete také psát kód toouse hello REST API nebo jeden z knihovny klienta úložiště tooperform hello těchto operací.

Jak je popsáno v části hello na hello [zabezpečení roviny Management](#management-plane-security), přístup toohello úložiště klíčů pro účet úložiště Classic lze udělit tím, že úplný přístup toohello předplatného Azure. Klíče úložiště toohello přístup pro účet úložiště pomocí modelu hello Azure Resource Manager můžete řídit pomocí řízení přístupu na základě Role (RBAC).

### <a name="how-toodelegate-access-tooobjects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Jak toodelegate přistupovat k tooobjects ve vašem účtu, který používá sdílené přístupové podpisy a uložené zásad přístupu
Sdíleného přístupového podpisu je řetězec, který obsahuje token zabezpečení, který může být připojen tooa identifikátor URI, který vám umožní toodelegate přístup toostorage objekty a zadejte omezení například hello oprávnění a rozsah hello data a času přístupu.

Můžete udělit přístup tooblobs, kontejnery, fronty zpráv, souborů a tabulek. S tabulkami můžete ve skutečnosti udělit oprávnění tooaccess rozsahu entit v tabulce hello zadáním hello oddílu a řádku rozsahy klíčů toowhich chcete hello uživatele toohave přístup. Například pokud máte data uložená se klíč oddílu zeměpisné stavu, může dáváte někdo přístup k datům hello toojust pro kalifornské.

Například můžete poskytnout webovou aplikaci token SAS, který mu umožní toowrite položky tooa fronty a poskytnout pracovní aplikace role SAS token tooget zprávy z hello fronty a jejich zpracování. Nebo jedno zákazníka může poskytnout token SAS, můžete použít tooupload obrázky tooa kontejner v úložišti objektů Blob a poskytnout tyto obrázky tooread oprávnění webové aplikace. V obou případech je oddělené oblasti zájmu – každá aplikace může být poskytnut přístup jenom hello, vyžadující v pořadí tooperform jejich úkolů. To je možné prostřednictvím použití hello podpisy sdíleného přístupu.

#### <a name="why-you-want-toouse-shared-access-signatures"></a>Proč chcete toouse sdílené přístupové podpisy
Proč byste měli toouse SAS místo právě poskytnutí klíč účtu úložiště, což je mnohem jednodušší? Poskytnutí klíč účtu úložiště je třeba sdílení klíčů hello království vašeho úložiště. Zajišťuje úplný přístup. Někdo může používat klíče a nahrajte svůj účet celé hudební knihovny tooyour úložiště. Můžou taky nahradit nakažené viry verzemi souborů, nebo ukrást vaše data. Poskytnutí rychle účet úložiště tooyour neomezený přístup je něco, co by neměly být lehce brány.

S podpisy sdíleného přístupu můžete udělit pouze hello oprávnění vyžadovaná pro omezené množství času klienta. Například pokud někdo odesílá účtu tooyour blob, můžete jim udělíte oprávnění k zápisu pro blob hello tooupload právě dostatek času (v závislosti na velikosti hello objektu hello blob samozřejmě). A pokud změníte své rozhodnutí, můžete tento přístup odvolat.

Kromě toho můžete určit, zda jsou požadavky pomocí SAS s omezeným přístupem tooa určité IP adresu nebo IP adresa rozsahu externí tooAzure. Můžete taky vyžadovat, že jsou vytvářeny požadavky pomocí konkrétní protokolu (HTTPS nebo HTTP nebo HTTPS). To znamená, pokud chcete tooallow komunikaci přes protokol HTTPS, lze nastavit pouze hello vyžaduje protokol tooHTTPS a přenos HTTP se zablokuje.

#### <a name="definition-of-a-shared-access-signature"></a>Definice sdílený přístupový podpis
Sdíleného přístupového podpisu je, že sadu parametrů dotazu připojí toohello URL ukazující na hello prostředků

který poskytuje informace o přístupu hello povolené a hello dobu, pro které hello je povolen přístup k. Tady je příklad; Tento identifikátor URI poskytuje tooa přístup pro čtení objektů blob pro pět minut. Všimněte si, že SAS parametrů dotazu musí být kódovaná v řetězci URL, například 3A % pro dvojtečkou (:) nebo % 20 prostoru.

```
http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL toohello blob)
?sv=2015-04-05 (storage service version)
&st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
&se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
&sr=b (resource is a blob)
&sp=r (read access)
&sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
&spr=https (only allow HTTPS requests)
&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for hello authentication of hello SAS)
```

#### <a name="how-hello-shared-access-signature-is-authenticated-by-hello-azure-storage-service"></a>Jak hello sdíleného přístupového podpisu je ověřována hello služba úložiště Azure
Když služba úložiště hello obdrží požadavek na hello, přebírá parametry hello vstupní dotaz a vytvoří podpis pomocí hello stejnou metodu, jak hello volací program. Pak porovná dvě podpisy hello. Pokud Souhlasím a potom hello službu úložiště můžete zkontrolujte hello úložiště služby verze toomake se, že je platný, ověřte, že hello aktuální datum a čas v rámci zadaného časového období hello, ujistěte se, požadovaný přístup hello odpovídá toohello požadavek atd.

Například s naše uvedenou adresu URL, pokud adresa URL hello byl odkazující tooa souboru místo v objektu blob, tento požadavek by nezdaří, protože určuje, že hello sdíleného přístupového podpisu je pro objekt blob. Pokud hello příkaz REST volané tooupdate objekt blob, by se nezdařila, protože hello sdíleného přístupového podpisu Určuje, zda je povoleno pouze pro čtení.

#### <a name="types-of-shared-access-signatures"></a>Druhy sdílených přístupových podpisů
* SAS úrovně služby lze použít tooaccess specifické prostředky v účtu úložiště. Příkladem takových načítání seznamu objektů BLOB v kontejneru, stahování objekt blob, aktualizaci entity v tabulce, přidáváte tooa fronty zpráv nebo odesílání souboru tooa sdílené složky.
* Úrovni účtu SAS lze použít tooaccess všechno, co SAS úrovně služby lze použít pro. Kromě toho se může poskytnout tooresources možnosti, které se nedá vymezit přes SAS úrovně služeb, jako je například hello možnost toocreate kontejnery, tabulky, fronty a sdílené složky. Můžete také zadat přístup toomultiple služby najednou. Například může dáváte někdo tooboth přístup k objektům BLOB a soubory ve vašem účtu úložiště.

#### <a name="creating-an-sas-uri"></a>Vytváření identifikátor URI SAS
1. Identifikátor URI ad hoc můžete vytvořit na vyžádání, definovat všechny parametry dotazu hello pokaždé, když.

   Toto je skutečně flexibilní, ale pokud máte logickou sadu parametrů, které jsou podobné pokaždé, když, pomocí zásad přístupu uložené je lepší představu.
2. Můžete vytvořit zásady přístupu uložené pro celý kontejneru, sdílené složky, tabulku nebo frontu. Pak je můžete použít jako základ hello pro hello identifikátory URI SAS, které vytvoříte. Oprávnění na základě zásad přístupu uložené dají snadno odvolat. Může mít až too5 zásad definovaných na každý kontejner, fronty, tabulka nebo sdílené složky.

   Například pokud se bude toohave Spousta lidí čtení objektů BLOB hello v konkrétním kontejneru, můžete vytvořit uložené zásady přístupu, která uvádí "udělit přístup pro čtení", a další nastavení, které budou stejné hello pokaždé, když. Potom můžete vytvořit identifikátor URI SAS pomocí nastavení hello hello uložené zásady přístupu a zadání vypršení platnosti hello datum a čas. Hello výhodou je, že nemáte toospecify všechny hello parametrů dotazu pokaždé, když.

#### <a name="revocation"></a>Odvolání
Předpokládejme, že došlo k ohrožení zabezpečení vaší SAS, nebo chcete toochange ho podnikové zabezpečení a dodržování legislativních požadavků. Jak můžete odvolat přístup tooa prostředků pomocí této SAS? To závisí na tom, jak jste vytvořili hello identifikátor URI pro SAS.

Pokud používáte ad hoc URI, máte tři možnosti. Může vystavovat tokeny SAS zásadám krátké vypršení platnosti a jednoduše počkejte hello SAS tooexpire. Můžete přejmenovat nebo odstranit prostředek hello (za předpokladu, že hello token jeden objekt oboru tooa). Hello klíče účtu úložiště, můžete změnit. Tato možnost poslední může mít velký dopad, v závislosti na tom, kolik služby jsou pomocí tohoto účtu úložiště a pravděpodobně není něco že chcete toodo bez některé plánování.

Pokud používáte SAS odvozené od zásadu přístupu uložené, můžete odebrat přístup k odvolání hello zásady přístupu uložené – můžete právě ho změnit tak, aby již vypršela platnost, nebo ji úplně odebrat. To se projeví okamžitě a zruší všechny SAS vytvořené pomocí této zásady přístupu uložené. Aktualizace nebo odebrání hello uložené zásady přístupu může mít vliv na uživatelé s přístupem k této konkrétní kontejner, sdílené složky, tabulka nebo fronty přes SAS, ale pokud hello klientů se zapisují, tak při hello starý stává neplatným požádají nové SAS, to bude pracovat správně.

Protože pomocí SAS odvozené od zásadu uložené přístupu vám dává toorevoke hello možnost, že SAS okamžitě, je, že hello doporučuje nejlepší postup tooalways používají uložené Pokud je to možné zásady přístupu.

#### <a name="resources"></a>Zdroje
Podrobnější informace o používání sdílené přístupové podpisy uložené zásady přístupu a dokončení s příklady naleznete v toohello následující články:

* Toto jsou články odkaz hello.

  * [SAS služby](https://msdn.microsoft.com/library/dn140256.aspx)

    Tento článek obsahuje příklady použití SAS úroveň služby s objekty BLOB, fronty zpráv, rozsahy tabulky a soubory.
  * [Vytváření SAS služby](https://msdn.microsoft.com/library/dn140255.aspx)
  * [Vytváření SAS účtu](https://msdn.microsoft.com/library/mt584140.aspx)
* Jedná se o kurzy pro použití hello .NET klienta knihovny toocreate sdílené přístupové podpisy a uložené zásady přístupu.

  * [Použití sdílených přístupových podpisů (SAS)](storage-dotnet-shared-access-signature-part-1.md)
  * [Sdílené přístupové podpisy, část 2: Vytvoření a použití SAS s hello služby objektů Blob](storage-dotnet-shared-access-signature-part-2.md)

    Tento článek obsahuje vysvětlení modelu SAS hello, příklady sdílené přístupové podpisy a doporučení pro hello osvědčený postup pomocí SAS. Je také popsána hello odvolání udělí oprávnění hello.
* Omezení přístupu pomocí IP adresy (IP ACL)

  * [Co je koncový bod seznamu řízení přístupu (ACL)?](../virtual-network/virtual-networks-acl.md)
  * [Vytváření SAS služby](https://msdn.microsoft.com/library/azure/dn140255.aspx)

    Toto je hello článku pro úroveň služby SAS; obsahuje příklad funkce ACLing IP.
  * [Vytváření SAS účtu](https://msdn.microsoft.com/library/azure/mt584140.aspx)

    Toto je odkaz na článek hello úrovni účtu SAS; obsahuje příklad funkce ACLing IP.
* Authentication

  * [Ověřování pro hello služby úložiště Azure](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* Sdílené přístupové podpisy kurzu Začínáme

  * [SAS kurzu Začínáme](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

## <a name="encryption-in-transit"></a>Šifrování během přenosu
### <a name="transport-level-encryption--using-https"></a>Šifrování transportní vrstvy – pomocí protokolu HTTPS
Další krok byste měli vzít tooensure hello zabezpečení vašich dat úložiště Azure je tooencrypt hello data mezi klientem hello a Azure Storage. první doporučuje Hello se tooalways používat hello [HTTPS](https://en.wikipedia.org/wiki/HTTPS) hello protokol, který zajišťuje zabezpečenou komunikaci prostřednictvím veřejného Internetu.

toohave zabezpečenou komunikaci, byste měli vždycky používat protokol HTTPS při volání metody hello rozhraní REST API nebo přístupu k nim objektů v úložišti. Navíc **sdílené přístupové podpisy**, může být použité toodelegate přístup k objektům úložiště tooAzure, zahrnout toospecify možnost této pouze hello protokol HTTPS můžete použít, pokud se používá sdílené přístupové podpisy, kontrolu, kdokoliv odesílání se propojení s tokeny SAS použije správný protokol hello.

Při volání metody hello rozhraní REST API tooaccess objekty v účtech úložiště, povolením můžete vynutit hello použití protokolu HTTPS [zabezpečení přenosu požadované](storage-require-secure-transfer.md) pro účet úložiště hello. Připojení pomocí protokolu HTTP bude odmítnuto, když je tato možnost povolena.

### <a name="using-encryption-during-transit-with-azure-file-shares"></a>Pomocí šifrování během přenosu se sdílenými složkami Azure
Úložiště Azure File podporuje protokol HTTPS, při použití hello REST API, ale je často používané jako sdílené složky SMB připojen tooa virtuálních počítačů. SMB 2.1 nepodporuje šifrování, takže připojení jsou povoleny pouze v rámci hello stejné oblasti v Azure. Ale protokolu SMB 3.0 podporuje šifrování a je k dispozici v systému Windows Server 2012 R2, Windows 8, Windows 8.1 a Windows 10, což mezi oblastmi přístup a dokonce i přístup na ploše hello.

Všimněte si, že i když se systémem Unix můžete použít sdílené složky Azure File, hello klient Linux SMB zatím nepodporuje šifrování, tak přístup je povoleno pouze v rámci oblasti Azure. Podpora šifrování pro Linux je na plán hello Linux vývojářů zodpovědná za funkce protokolu SMB. Při přidávání šifrování, máte stejné možnosti pro přístup k Azure sdílenou v systému Linux, jako je tomu u Windows hello.

Hello použití šifrování s hello službu Azure souborů můžete vynutit tím, že umožňuje [zabezpečení přenosu požadované](storage-require-secure-transfer.md) pro účet úložiště hello. Pokud pomocí hello rozhraní REST API, je požadován protokol HTTPs. Pouze připojení protokolu SMB, která podporují šifrování protokolu SMB, bude připojovat úspěšně.

#### <a name="resources"></a>Zdroje
* [Jak toouse Azure File storage s Linuxem](storage-how-to-use-files-linux.md)

  Tento článek ukazuje, jak sdílet toomount Azure File na Linux systému a nahrávání nebo stahování souborů.
* [Začínáme s úložištěm Azure File ve Windows](storage-dotnet-how-to-use-files.md)

  Tento článek nabízí přehled Azure sdílené složky a jak toomount a použít je pomocí prostředí PowerShell a rozhraní .NET.
* [Uvnitř Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)

  Tento článek ohlášen hello obecné dostupnosti úložiště Azure File a obsahuje technické informace o šifrování hello protokolu SMB 3.0.

### <a name="using-client-side-encryption-toosecure-data-that-you-send-toostorage"></a>Pomocí data toosecure šifrování na straně klienta, odešlete toostorage
Jinou možnost, která vám pomůže zajistit, aby vaše data byla zabezpečené při přenášení mezi klientskou aplikaci a úložiště je šifrování na straně klienta. Hello data se šifrují před přenášením do úložiště Azure. Při načítání hello data ze služby Azure Storage, hello data se dešifrují po přijetí na straně klienta hello. I když hello data se šifrují, budete napříč přenosová hello, doporučujeme také používat protokol HTTPS, protože má kontrolu integrity dat součástí, které vám pomůžou snížit chyby sítě by to ovlivnilo hello integritu dat hello.

Šifrování na straně klienta je také metodu šifrování dat v klidovém stavu, jako je hello data uložená v šifrované podobě. Budeme mluvit o to podrobněji v části hello na [šifrování v klidovém stavu](#encryption-at-rest).

## <a name="encryption-at-rest"></a>Šifrování v klidovém stavu
Existují tři Azure funkce, které poskytují šifrování v klidovém stavu. Azure Disk Encryption je použité tooencrypt hello operačního systému a datové disky v virtuální počítače IaaS. Další dva Hello – šifrování na straně klienta a SSE – jsou data i tooencrypt používaných ve službě Azure Storage. Pojďme podívejte se na každý z nich a pak neprovádějte porovnání a při každé z nich používá.

I když můžete pomocí šifrování na straně klienta tooencrypt hello dat během přenosu (který je také uložený v zašifrované podobě v úložišti), může při přenosu hello dáváte přednost toosimply použití protokolu HTTPS a mají některé způsob, jak toobe data hello automaticky šifrují, pokud je uložené. Existují dva způsoby toodo to--Azure Disk Encryption a SSE. Jeden slouží toodirectly šifrování dat hello na operačního systému a datové disky, které jsou používány virtuálními počítači a hello jiných je použité tooencrypt dat zapsaných tooAzure úložiště objektů Blob.

### <a name="storage-service-encryption-sse"></a>Šifrování služby úložiště (SSE)
SSE umožňuje toorequest, že služba úložiště hello automaticky šifrovat hello data při zápisu ho tooAzure úložiště. Při čtení dat hello ze služby Azure Storage, ji budou dešifrovat službou úložiště hello před vrácením. To vám umožní toosecure data bez nutnosti toomodify kódu nebo přidat kód tooany aplikace.

Je toto nastavení, která se použije účet toohello celou úložiště. Můžete povolit nebo zakázat tuto funkci tak, že změníte hodnotu hello hello nastavení. toodo, můžete použít hello hello portál Azure, prostředí PowerShell, rozhraní příkazového řádku Azure, hello REST API poskytovatele prostředků úložiště nebo hello Klientská knihovna pro .NET úložiště. Ve výchozím nastavení je SSE vypnutý.

V tomto okamžiku hello klíče pro šifrování hello spravuje Microsoft. Jsme generovat klíče hello původně a spravovat hello zabezpečeného úložiště hello klíče, jakož i regulární otočení hello podle definice interní zásady společnosti Microsoft. V budoucích hello získat hello možnost toomanage šifrovacích klíčů a poskytnutí cesty migrace z klíče spravované Microsoftem spravované toocustomer klíčů.

Tato funkce je k dispozici pro Standard a Premium Storage účty, které jsou vytvořené pomocí modelu nasazení Resource Manager hello. SSE platí pouze tooblock BLOB, objekty BLOB stránky a doplňovací objekty BLOB. Hello dalších typů dat, včetně tabulek, front a soubory, nebudou šifrována.

Data jsou zašifrována, pouze pokud je zapnuto SSE a hello data se zapisují tooBlob úložiště. Povolení nebo zakázání SSE nemá negativní vliv na existující data. Jinými slovy Pokud povolíte toto šifrování, se nebude přejděte zpět a šifrovat data, která již existuje; ani bude dešifrovat hello data, která už při zakázání SSE.

Pokud chcete tuto funkci v účtu úložiště Classic toouse, můžete vytvořit nový účet úložiště Resource Manager a použít AzCopy toocopy hello data toohello nový účet.

### <a name="client-side-encryption"></a>Šifrování na straně klienta
Jsme uvedených šifrování na straně klienta, když hovoříte o hello šifrování přenášených dat hello. Tato funkce umožňuje vám tooprogrammatically šifrování dat v aplikaci klienta před odesláním napříč toobe přenosová hello zapsána tooAzure úložiště a tooprogrammatically dešifrování dat po načtení z úložiště Azure.

To poskytuje šifrování během přenosu, ale také poskytuje funkce hello šifrování v klidovém stavu. Všimněte si, že i když hello data jsou zašifrována během přenosu, doporučujeme pomocí protokolu HTTPS tootake výhod kontrolu integrity dat předdefinované hello, které pomůžou zmírnit by to ovlivnilo hello integritu dat hello chyby sítě.

Příklad kde tuto funkci můžete použít je, pokud máte webovou aplikaci, která ukládá objekty BLOB a načte objekty BLOB a chcete hello aplikace a data toobe co možná. V takovém případě byste použili šifrování na straně klienta. Hello provozu mezi klientem hello a hello služby objektů Blob Azure obsahuje prostředek hello šifrované a nikdo interpretovat hello přenášených dat a rekonstruovat do privátní objektů BLOB.

Šifrování na straně klienta jsou součástí hello Java a hello .NET úložiště klientské knihovny, které pak použijte hello klíč trezoru rozhraní API Správce Azure, což pro tooimplement je velmi snadné. Hello proces šifrování a dešifrování dat hello používá hello obálky technik a ukládá metadata používá šifrování hello v každém objektu úložiště. Například pro objekty BLOB, ukládají se v hello metadata objektu blob, zatímco pro fronty, přidá se tooeach fronty zpráv.

Pro šifrování hello, sám může vygenerovat a spravovat vlastní šifrovací klíče. Můžete také použít klíče generované hello Klientská knihovna pro úložiště Azure, nebo můžete mít hello Azure Key Vault generovat klíče hello. Můžete uložit šifrovací klíče ve vaší místní úložiště klíčů a uložit je do Azure Key Vault. Azure Key Vault umožňuje toogrant přístup toohello tajné klíče v Azure Key Vault toospecific uživatelů pomocí služby Azure Active Directory. To znamená, že nejen Kdokoliv může číst hello Azure Key Vault a načtení hello klíčů, které používáte pro šifrování na straně klienta.

#### <a name="resources"></a>Zdroje
* [Šifrování a dešifrování objektů BLOB v úložišti Microsoft Azure pomocí Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md)

  Tento článek ukazuje, jak šifrování na straně klienta toouse s Azure Key Vault, jak toocreate hello KEK a uloží jej v trezoru hello pomocí prostředí PowerShell.
* [Šifrování na straně klienta a Azure Key Vault pro Microsoft Azure Storage](storage-client-side-encryption.md)

  Tento článek vysvětluje šifrování na straně klienta a obsahuje příklady použití úložiště hello klienta knihovny tooencrypt a dešifrování prostředky z hello čtyři služby úložiště. Také pojednává o Azure Key Vault.

### <a name="using-azure-disk-encryption-tooencrypt-disks-used-by-your-virtual-machines"></a>Pomocí Azure Disk Encryption tooencrypt disky používat virtuální počítače
Azure Disk Encryption je nová funkce. Tato funkce umožňuje tooencrypt hello OS disky a datové disky použít podle IaaS virtuálního počítače. Pro systém Windows jsou šifrované jednotky hello pomocí standardních technologií šifrování nástroje BitLocker. Pro systémy Linux jsou disky hello šifrované pomocí hello DM-Crypt technologie. Toto je integrovaná s Azure Key Vault tooallow jste toocontrol a správu hello disku šifrovacích klíčů.

řešení Hello podporuje následující scénáře pro virtuální počítače IaaS, pokud jsou povolené v Microsoft Azure hello:

* Integrace s Azure Key Vault
* Úroveň Standard virtuálních počítačů: [A, D, DS, G, GS a atd. řady virtuální počítače IaaS](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Povolení šifrování v systému Windows a virtuálních počítačů IaaS Linux
* Zakázáním šifrování na operačního systému a datové disky pro virtuální počítače IaaS Windows
* Zakázáním šifrování na datových jednotkách pro virtuální počítače IaaS Linux
* Povolení šifrování na virtuální počítače IaaS se systémem Windows klientského operačního systému
* Povolení šifrování u svazků s připojení cesty
* Povolení šifrování u virtuálních počítačů Linux, které jsou nakonfigurované s diskem s použitím mdadm prokládání (RAID)
* Povolení šifrování na virtuální počítače s Linuxem pomocí LVM pro datové disky
* Povolení šifrování na virtuálních počítačích Windows, které jsou nakonfigurované pomocí prostorů úložiště
* Jsou podporovány všechny veřejné oblasti Azure

řešení Hello nepodporuje následující scénáře, funkce a technologie v hello verzi hello:

* Úroveň Basic virtuálních počítačů IaaS
* Zakázáním šifrování na jednotce operačního systému pro virtuální počítače IaaS Linux
* Virtuální počítače IaaS, vytvořené pomocí metody vytvoření virtuálního počítače classic hello
* Integrace s vaší místní služby správy klíčů
* Úložiště Azure File (systém souborů sdíleného), systém souborů NFS, dynamické svazky a virtuální počítače Windows, které jsou nakonfigurované s systémy na bázi softwaru diskového pole RAID


> [!NOTE]
> Hello následující Linuxových distribucích aktuálně podporuje šifrování disku operačního systému Linux: RHEL 7.2, CentOS 7.2n a Ubuntu 16.04.
>
>

Tato funkce zajišťuje, že všechna data na disky virtuálního počítače je zašifrovaná přinejmenším ve službě Azure Storage.

#### <a name="resources"></a>Zdroje
* [Azure Disk Encryption pro systém Windows a virtuálních počítačů Linux IaaS](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)

### <a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Porovnání Azure Disk Encryption, SSE a šifrování na straně klienta
#### <a name="iaas-vms-and-their-vhd-files"></a>Virtuální počítače IaaS a jejich soubory virtuálního pevného disku
Pro disky používané virtuální počítače IaaS doporučujeme používat Azure Disk Encryption. Můžete zapnout SSE tooencrypt hello virtuálního pevného disku soubory, které jsou používané tooback těchto disků ve službě Azure Storage, ale šifruje pouze nově vytvořené data. To znamená, že pokud vytvoření virtuálního počítače a pak povolíte SSE hello účtu úložiště, která obsahuje soubor virtuálního pevného disku hello, budou zašifrovány pouze hello změny, není hello původní soubor virtuálního pevného disku.

Pokud vytvoříte virtuální počítač pomocí bitové kopie z hello Azure Marketplace, provede Azure [Nedávná kopie](https://en.wikipedia.org/wiki/Object_copying) z bitové kopie tooyour hello nejsou šifrována účet úložiště v Azure Storage ale i v případě, že máte SSE povolena. Po hello virtuálního počítače vytvoří a spustí aktualizaci bitové kopie hello, začne SSE šifrování dat hello. Z tohoto důvodu je nejlepší toouse Azure Disk Encryption na virtuálních počítačích vytvořen z bitové kopie v Azure Marketplace hello, pokud chcete, aby je plně zašifrované.

Pokud předem šifrované virtuální počítač se přenést do Azure z místní, budete se moct tooupload hello šifrovací klíče tooAzure Key Vault a pokračovat v používání hello šifrování pro tento virtuální počítač, že jste používali místní. Azure Disk Encryption je povoleno toohandle tento scénář.

Pokud máte bez šifrování virtuálního pevného disku z místně, můžete odeslat do Galerie hello jako vlastní image a zřídit virtuální počítač z něj. Pokud to uděláte pomocí šablony Resource Manageru hello, požádejte ho tooturn na Azure Disk Encryption při spuštění hello virtuálních počítačů.

Když přidáte datový disk a připojte na hello virtuálních počítačů, můžete zapnout na Azure Disk Encryption na tento datový disk. Zašifruje tento disk data místně nejprve a vrstva správy služby hello bude proveďte opožděné zápisu proti úložiště tak obsah úložiště hello je šifrován.

#### <a name="client-side-encryption"></a>Šifrování na straně klienta
Šifrování na straně klienta je hello nejbezpečnější metodu šifrování dat, protože se šifruje před přenosu který šifruje hello data v klidovém stavu. Ale vyžadovat, že přidáte kód tooyour aplikací pomocí úložiště, které nemusí být žádoucí toodo. V těchto případech můžete použít protokol HTTPs pro vaše data při přenosu a SSE tooencrypt hello dat v klidovém stavu.

Pomocí šifrování na straně klienta můžete šifrovat entity tabulky, fronty zpráv a objekty BLOB. S SSE můžete šifrovat jenom objekty BLOB. Pokud potřebujete tabulky a fronty toobe data šifrovat, měli byste použít šifrování na straně klienta.

Šifrování na straně klienta je kompletně spravované službou hello aplikace. Toto je nejbezpečnější přístup hello, ale vyžadují toomake programové změny tooyour aplikace a umístí procesy správy klíčů na místě. To byste použili, když chcete, aby hello další bezpečnostní během přenosu a vy chcete toobe vaše uložená data zašifrovaná.

Šifrování na straně klienta je další zátěž hello klienta, a máte tooaccount pro tento v plánu škálovatelnost, zejména v případě, že jsou šifrování a přenosu velkého množství dat.

#### <a name="storage-service-encryption-sse"></a>Šifrování služby úložiště (SSE)
SSE spravuje Azure Storage. Pomocí SSE neposkytuje pro zabezpečení hello hello dat během přenosu, ale jeho šifrování dat hello zapsané tooAzure úložiště. Neexistuje žádný vliv na výkon hello, při použití této funkce.

Můžete pouze šifrování objekty BLOB bloku, doplňovací objekty BLOB a objektům BLOB pomocí SSE stránky. Pokud potřebujete tooencrypt tabulka data nebo data fronty, měli byste zvážit použití šifrování na straně klienta.

Pokud máte archiv nebo knihovna soubory virtuálního pevného disku, které můžete použít jako základ pro vytváření nových virtuálních počítačů, můžete vytvořit nový účet úložiště, povolit SSE a pak nahrajte účet toothat soubory virtuálního pevného disku hello. Tyto soubory virtuálního pevného disku, bude se šifrovat úložiště Azure.

Pokud máte Azure Disk Encryption povolené pro hello disky ve virtuálních počítačů a SSE povoleno na účet úložiště hello podržíte hello soubory virtuálního pevného disku, bude pracovat správně; způsobí žádná data pro nově vytvořený šifrovaný dvakrát.

## <a name="storage-analytics"></a>Storage Analytics
### <a name="using-storage-analytics-toomonitor-authorization-type"></a>Pomocí Storage Analytics toomonitor autorizace typu
Pro každý účet úložiště můžete povolit protokolování tooperform Azure Storage Analytics a ukládat data metriky. Toto je skvělý nástroj toouse při toocheck hello metriky výkonu účet úložiště, nebo se vyžadují tootroubleshoot účet úložiště, protože dochází k problémům s výkonem.

Další část dat, která se zobrazí v hello úložiště analýzy protokolů je metoda ověřování hello používá někdo při přístupu k úložišti. Například pomocí úložiště objektů Blob, uvidíte pokud používají sdíleného přístupového podpisu nebo hello klíče účtu úložiště, nebo pokud byl objekt blob hello získat přístup k veřejné.

To může být velmi užitečné, pokud jsou úzce ochrana toostorage přístup. V úložišti objektů Blob můžete například nastavit všechny kontejnery tooprivate hello a implementovat hello použití služby SAS v celé vaší aplikace. Zkontrolujte, zda text hello protokoly pravidelně toosee Pokud objektů BLOB ke kterým se přistupuje pomocí klíčů k účtu úložiště hello, které mohou indikovat porušení zabezpečení, nebo pokud jsou objekty BLOB hello veřejné, ale nemělo by být.

#### <a name="what-do-hello-logs-look-like"></a>Co hello protokoly vypadat?
Po povolení hello úložiště účet metriky a protokolování prostřednictvím hello portálu Azure, analytická data budou tooaccumulate rychle začít. Hello protokolování a metriky pro každou službu je samostatný; protokolování Hello je zapisovány pouze v případě, že je aktivita v daném účtu úložiště, zatímco hello metriky se zaznamená do protokolu, každou minutu, každou hodinu nebo každý den, v závislosti na tom, jak ho nakonfigurujete.

Hello protokoly se ukládají do objektů BLOB bloku v kontejneru nazvaném $logs v účtu úložiště hello. Tento kontejner se automaticky vytvoří, když je povolená analytika úložiště. Po vytvoření tohoto kontejneru, nelze odstranit i když můžete odstranit jeho obsah.

V kontejneru hello $logs nachází složka pro každou službu a pak podsložky nejsou hello rok/měsíc/den/hodinu. Necelé hodiny jsou jednoduše číslované hello protokoly. Toto je co hello bude vypadat adresářovou strukturu:

![Zobrazení souborů protokolu](./media/storage-security-guide/image1.png)

Každý požadavek tooAzure úložiště přihlášen. Zde je snímek souboru protokolu, zobrazující hello první několik polí.

![Snímek souboru protokolu](./media/storage-security-guide/image2.png)

Uvidíte, které můžete použít hello protokoly tootrack jakýkoli druh účtu úložiště tooa volání.

#### <a name="what-are-all-of-those-fields-for"></a>Jaké jsou všechna tyto pole pro?
Je uvedené v hello prostředky níže článek, který poskytuje hello seznam hello mnoho polí na hello protokoly a jejich použití. Tady je seznam hello polí v pořadí:

![Snímek polí v souboru protokolu](./media/storage-security-guide/image3.png)

Zajímají nás hello položky getblob – a jak jsou ověřeni, takže jsme toolook nutnost položky s typ operace "Get-Blob" a zkontrolujte hello – stav žádosti o (4<sup>tý</sup> sloupec) a hello autorizace – typ (8<sup>tý</sup> sloupce).

Například v hello nejprve několik řádků v seznamu hello výše, hello žádost – stav je "ÚSPĚCH" a hello autorizace type je "ověřit". To znamená, že žádost o hello se ověřila pomocí hello klíč účtu úložiště.

#### <a name="how-are-my-blobs-being-authenticated"></a>Jak jsou ověřovaného Moje objekty BLOB?
Máme tři případů, které jsme zájem.

1. Objekt blob Hello je veřejná a je přístupný pomocí adresy URL bez sdíleného přístupového podpisu. V takovém případě hello žádost – stav je "AnonymousSuccess" a je "anonymní" hello autorizace type.

   1.0; 2015-11-17T02:01:29.0488963Z; Getblob –; **AnonymousSuccess**, 200, 124, 37; **Anonymní**; mystorage...
2. Objekt blob Hello je privátní a se použila se sdíleným přístupovým podpisem. V takovém případě je hello žádost status "SASSuccess" a hello autorizace – typ je "sas".

   1.0; 2015-11-16T18:30:05.6556115Z; Getblob –; **SASSuccess**, 200, 416, 64; **SAS**; mystorage...
3. Objekt blob Hello je privátní a klíč úložiště hello byl použité tooaccess ho. V takovém případě je hello žádost status "**úspěch**"a je hello autorizace type"**ověřený**".

   1.0; 2015-11-16T18:32:24.3174537Z; Getblob –; **Úspěch**; 206; 59; 22; **ověření**; mystorage...

Můžete použít Microsoft Message Analyzer tooview hello a tyto protokoly analyzovat. Obsahuje možnosti Hledat a filtr. Například můžete chtít toosearch pro instance toosee getblob – Pokud je využití hello co můžete očekávat, tj. toomake zda někdo není přístup k účtu úložiště nesprávně.

#### <a name="resources"></a>Zdroje
* [Storage Analytics](storage-analytics.md)

  Tento článek je přehled analytika úložiště a jak tooenable je.
* [Formát úložiště analýzy protokolů](https://msdn.microsoft.com/library/azure/hh343259.aspx)

  Tento článek ukazuje hello formát úložiště analýzy protokolů a podrobnosti hello dostupná pole v něm, včetně ověřování typu, který označuje hello typ ověřování používá pro žádost hello.
* [Monitorování účtu úložiště v hello portálu Azure](storage-monitor-storage-account.md)

  Tento článek ukazuje, jak tooconfigure sledování metrik a protokolování pro účet úložiště.
* [Začátku do konce Poradce při potížích s použitím Azure Storage Metrics a protokolování, AzCopy a Message Analyzer](storage-e2e-troubleshooting.md)

  Tento článek pojednává o řešení problémů pomocí hello Storage Analytics a ukazuje, jak toouse hello Microsoft Message Analyzer.
* [Microsoft Message Analyzer operační Průvodce](https://technet.microsoft.com/library/jj649776.aspx)

  Tento článek je hello odkaz pro hello Microsoft Message Analyzer a obsahuje odkazy tooa kurzu, rychlý start a Souhrn funkcí.

## <a name="cross-origin-resource-sharing-cors"></a>Sdílení prostředků mezi zdroji (CORS)
### <a name="cross-domain-access-of-resources"></a>Přístup do jiných domén prostředků
Při spuštění v jedné doméně webový prohlížeč provede požadavek HTTP pro prostředek z jiné domény, se nazývá žádosti HTTP nepůvodního zdroje. Například stránky HTML z contoso.com odešle požadavek pro hostované na fabrikam.blob.core.windows.net jpeg. Z bezpečnostních důvodů omezit prohlížeče HTTP žádostí napříč zdroji spustily z funkce skripty, jako je JavaScript. To znamená, že pokud určitý kód JavaScript na webové stránce v doméně contoso.com požádá o tomto jpeg na fabrikam.blob.core.windows.net, prohlížeč hello nebude hello požadavku.

Co to znamená mít toodo s Azure Storage? Dobře, pokud ukládáte statické prostředky, jako jsou soubory dat JSON nebo XML v Blob Storage pomocí účtu úložiště volána Fabrikam, hello doménu pro prostředky hello bude fabrikam.blob.core.windows.net a hello contoso.com webová aplikace nebude možné tooaccess je pomocí jazyka JavaScript, protože hello domény se liší. Toto je hodnota true, pokud se snažíte toocall mezi hello služby úložiště Azure – například Table Storage –, které vracejí toobe data JSON zpracovává hello JavaScript klienta.

#### <a name="possible-solutions"></a>Možná řešení
Jedním ze způsobů tooresolve jde tooassign vlastní doménu jako toofabrikam.blob.core.windows.net "storage.contoso.com". Hello problém je, že lze přiřadit pouze tento účet úložiště tooone vlastní doménu. Co dělat, když jsou uložené prostředky hello ve více účtů úložiště?

Jiný způsob tooresolve jde toohave hello webové aplikace act jako proxy pro volání hello úložiště. To znamená, pokud nahráváte soubor tooBlob úložiště, hello webová aplikace by buď zapisuje na místním počítači a potom ho zkopírujete tooBlob úložiště, nebo by všechny načtení do paměti a zapište tooBlob úložiště. Alternativně můžete napsat vyhrazené webové aplikace (například webového rozhraní API), která odešle hello soubory místně a zapíše tooBlob úložiště. V obou případech máte tooaccount pro tuto funkci při určení škálovatelnost hello potřebuje.

#### <a name="how-can-cors-help"></a>Jak může pomoci CORS?
Úložiště Azure vám umožní tooenable CORS – křížové sdílení prostředků původu. Pro každý účet úložiště můžete zadat domén, které mají přístup k prostředkům hello v daném účtu úložiště. Například v našem případě uvedených výše jsme můžete zapnout CORS na účet úložiště fabrikam.blob.core.windows.net hello a nakonfigurovat ho toocontoso.com tooallow přístup. Potom hello webové aplikace contoso.com přímo přístup k prostředkům hello ve fabrikam.blob.core.windows.net.

Jednu věc toonote je, že CORS umožňuje přístup, ale neposkytuje ověřování, který je nutný pro všechny jiný veřejný přístup k prostředkům úložiště. To znamená, že přístupné pouze objekty BLOB, pokud jsou veřejné nebo zahrnete sdíleného přístupového podpisu budete hello příslušná oprávnění. Tabulky, fronty a soubory nemají veřejný přístup a vyžadují SAS.

Ve výchozím nastavení je CORS na všechny služby zakázána. CORS můžete povolit pomocí hello REST API nebo hello úložiště klienta knihovny toocall jednu z hello metody tooset hello služby zásad. Pokud můžete udělat, můžete zahrnout CORS pravidlo, které je v kódu XML. Tady je příklad pravidla CORS, která byla nastavena pomocí operace hello nastavit vlastnosti služby pro hello služby objektů Blob pro účet úložiště. Můžete provést tuto operaci pomocí klientské knihovny pro úložiště hello nebo hello rozhraní REST API pro Azure Storage.

```xml
<Cors>    
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
        <MaxAgeInSeconds>200</MaxAgeInSeconds>
    </CorsRule>
<Cors>
```

Tady je co znamená každý řádek:

* **AllowedOrigins** sdělí které neodpovídající domény můžete požadovat a přijímat data ze služby úložiště hello. To říká, že contoso.com a fabrikam.com můžete data žádosti z úložiště objektů Blob pro účet konkrétní úložiště. Můžete také nastavit tento tooa zástupný znak (\*) tooallow všechny domény tooaccess požadavky.
* **AllowedMethods** je hello seznam metod (příkazy požadavku HTTP), které lze použít při vytváření požadavku hello. V tomto příkladu jsou povoleny pouze PUT a GET. Můžete nastavit tento tooa zástupný znak (\*) tooallow všechny metody toobe použít.
* **AllowedHeaders** Toto je požadavek hello hlavičky, které hello zdrojovou doménu můžete zadat při vytváření požadavku hello. V tomto příkladu jsou povoleny všechny hlavičky metadata počínaje x-ms-meta-data x-ms-meta cíl a x-ms-meta-abc. Hello zástupný znak (\*) znamená, že všechny hlavičky počínaje hello určeného předpona je povolen.
* **ExposedHeaders** to informuje o tom, které hlavičky odpovědi by měly být vystaveny vystavitelem požadavek toohello prohlížeče hello. V tomto příkladu všechny hlavičky počínaje "x-ms - meta-" k dispozici.
* **MaxAgeInSeconds** jde hello maximální množství času, že prohlížeč bude mezipaměti předběžný požadavek možnosti hello. (Další informace o předběžný požadavek hello, zkontrolujte, zda text hello první článek.)

#### <a name="resources"></a>Zdroje
Další informace o CORS a jak tooenable, podrobnosti naleznete v těchto prostředků.

* [Podpora sdílení prostředků různých původů (CORS) pro hello úložiště služby Azure na Azure.com.](storage-cors-support.md)

  Tento článek obsahuje přehled CORS a jak tooset hello pravidel pro služby jiného úložiště hello.
* [Podpora sdílení prostředků různých původů (CORS) pro hello úložiště služby Azure na webu MSDN](https://msdn.microsoft.com/library/azure/dn535601.aspx)

  Toto je hello referenční dokumentaci k nástroji pro podporu CORS pro služby úložiště Azure hello. To má odkazy tooarticles použití tooeach služba úložiště a slouží jako příklad a vysvětluje každý prvek v souboru CORS hello.
* [Microsoft Azure Storage: Úvod CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

  Toto je odkaz toohello počáteční blog článek uvedení CORS a zobrazuje jak toouse ho.

## <a name="frequently-asked-questions-about-azure-storage-security"></a>Nejčastější dotazy o zabezpečení Azure Storage
1. **Jak můžete ověřit integritu hello hello objekty BLOB, které I mě přenosu do nebo z Azure Storage, když nelze použít protokol HTTPS hello?**

   Pokud z jakéhokoli důvodu, že potřebujete toouse HTTP místo protokolu HTTPS a práci s objekty BLOB bloku, můžete použít kontrolu MD5 toohelp Ověřte integritu hello objektů BLOB hello přenášení. To vám pomůže s ochranou z chyb sítě/transport layer, ale nemusí nutně jít s zprostředkující útoky.

   Pokud použijete protokol HTTPS, který poskytuje zabezpečení na úrovni přenosu, pak pomocí MD5 kontrola je redundantní a zbytečné.

   Další informace najdete v článku hello [přehled MD5 objektu Blob Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).
2. **Co o FIPS – dodržování předpisů pro USA hello Státní?**

   Hello Spojených států informace o zpracování FIPS (Federal Standard) definuje kryptografické algoritmy pro použití schváleno USA Federální vládou počítačích pro ochranu hello citlivá data. Povolení FIPS režimu na Windows serveru nebo plochy informuje hello OS že má být použita pouze ověřených algoritmem FIPS kryptografické algoritmy. Pokud aplikace používá nevyhovující algoritmy, dojde k přerušení aplikace hello. With.NET Framework verze 4.5.2 nebo vyšší, hello aplikace se automaticky nepřepne hello kryptografické algoritmy kompatibilní se standardem FIPS toouse algoritmy při hello počítač se nachází v režimu FIPS.

   Microsoft zůstává až tooeach zákazníka toodecide jestli tooenable režimu FIPS. Věříme, že neexistuje žádný důvod poutavé pro zákazníky, kteří nejsou v režimu FIPS subjektu toogovernment předpisy tooenable ve výchozím nastavení.

   **Prostředky**

* [Proč se není doporučujeme "Režimu FIPS" už](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

  Tento článek na blogu shrnovat standardu FIPS a vysvětluje, proč nemáte umožňují režimu FIPS ve výchozím nastavení.
* [FIPS 140 ověření](https://technet.microsoft.com/library/cc750357.aspx)

  Tento článek obsahuje informace o tom, jak produkty společnosti Microsoft a kryptografické moduly v souladu se standardem FIPS hello pro USA hello Federální vládou.
* ["Kryptografie systému: použití standardu FIPS kompatibilní s algoritmy pro šifrování, hašování a podpisování" důsledky nastavení zabezpečení v systému Windows XP a v novějších verzích systému Windows](https://support.microsoft.com/kb/811833)

  V tomto článku bude zmíněn hello použití režimu FIPS ve starší počítače se systémem Windows.
