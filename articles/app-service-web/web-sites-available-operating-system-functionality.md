---
title: "funkci systému aaaOperating v Azure App Service"
description: "Další informace o hello OS funkce dostupné tooweb aplikace, back-EndY mobilních aplikací a aplikace API v Azure App Service"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 39d5514f-0139-453a-b52e-4a1c06d8d914
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: 695188c48102b10ece326fba8d50a5f55b03b003
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="operating-system-functionality-on-azure-app-service"></a>Funkce operačního systému v Azure App Service
Tento článek popisuje hello běžné směrného plánu operačního systému funkce, které jsou k dispozici tooall aplikace běžící na [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Tato funkce zahrnuje soubor, sítě a přístup k registru a diagnostické protokoly a události. 

<a id="tiers"></a>

## <a name="app-service-plan-tiers"></a>Úrovně plánu služby App Service
Služby App Service zákazníka aplikace běží v hostitelských prostředí s více klienty. Aplikace nasazené v hello **volné** a **sdílené** vrstev spouštět v pracovních procesů na sdílených virtuálních počítačích, při aplikace nasazené v hello **standardní** a  **Premium** vrstev spustit na virtuální počítače vyhrazené speciálně pro aplikace hello související s jednoho zákazníka.

Protože služby App Service podporuje bezproblémové škálování prostředí mezi různých vrstev, konfigurace zabezpečení hello vynucuje pro hello zůstane aplikace služby App Service stejné. Tím se zajistí, že aplikace není chovat najednou jinak, selhání neočekávané způsoby, když plán služby App Service přepíná z jedné vrstvy tooanother.

<a id="developmentframeworks"></a>

## <a name="development-frameworks"></a>Architektury pro vývoj
App Service cenové úrovně řízení hello množství výpočetních prostředků (procesor, diskové úložiště, paměti a odchozí síťový) k dispozici tooapps. Hello spektra framework funkce dostupné tooapps však zůstane stejný bez ohledu na to hello škálování vrstvy hello.

App Service podporuje celou řadu architektury pro vývoj, včetně ASP.NET, klasické ASP, node.js, PHP a Python - všechny spouští jako rozšíření v rámci služby IIS. V pořadí toosimplify a normalizaci konfigurace zabezpečení, aplikacemi App Service obvykle spusťte hello různé architektury pro vývoj ve výchozím nastavení. Jeden ze způsobů tooconfiguring aplikace může byly útoku na toocustomize hello rozhraní API a funkce pro každé rozhraní pro jednotlivé vývoj. Služby App Service místo trvá více obecné přístup povolením běžné základní funkce operačního systému bez ohledu na to rozhraní pro vývoj aplikace.

Hello následující oddíly shrnují hello obecné typy aplikace služby k dispozici tooApp funkce operačního systému.

<a id="FileAccess"></a>

## <a name="file-access"></a>Přístup k souborům
Existují různé jednotky v App Service, včetně místní jednotky a síťové jednotky.

<a id="LocalDrives"></a>

### <a name="local-drives"></a>Místní jednotky
Jádro aplikace App Service je služby spuštěné na infrastruktuře hello Azure PaaS (platforma jako služba). Jako výsledek, hello místní disky, které jsou tooa virtuální počítač "připojené" hello stejné role pracovního procesu typy k dispozici tooany jednotky běží v Azure. To zahrnuje jednotce operačního systému (hello jednotku D:\), aplikaci jednotku, která obsahuje soubory cspkg balíčku Azure používá výhradně služby App Service (a nepřístupný toocustomers) a na jednotku "user" (hello jednotka C:\), jejichž velikost se liší v závislosti na velikosti hello Dobrý den virtuálních počítačů.

<a id="NetworkDrives"></a>

### <a name="network-drives-aka-unc-shares"></a>Síťové jednotky (neboli UNC sdílené složky)
Jedním z hello jedinečné aspekty aplikační služba, která usnadňuje nasazení aplikace a údržby přehledné je, že je všechny uživatele obsah uložený na sadu sdílené složky UNC. Tento model mapuje velmi vhodně toohello běžný vzor obsahu úložiště používá místní webových hostitelských prostředích, která mají víc serverů s vyrovnáváním zatížení. 

V rámci služby App Service se počet UNC sdílené složky vytvořené v každém datovém centru. Procento hello obsah uživatele pro všechny zákazníky v každé datového centra je přidělen tooeach sdílené složky UNC. Kromě toho všechny hello obsahu souboru pro jednoho zákazníka předplatného je umístěn vždy na hello sdílet stejnou UNC. 

Všimněte si, že kvůli toohow cloudové služby fungovat, hello konkrétní virtuální počítač za hostování sdílené složky UNC se časem změnit. Je zaručeno, že sdílené složky UNC se připojí jiné virtuální počítače, jako přechodem během normálního chodu operací cloudu hello nahoru a dolů. Z tohoto důvodu by měla aplikace umožňují nikdy pevně předpoklady, které hello informace o počítači v cestě k souboru UNC zůstanou stabilní v čase. Místo toho měli používat hello pohodlný *umělé* absolutní cestu **D:\home\site** poskytující služby App Service. Tato umělé absolutní cesta poskytuje přenosné, aplikace a uživatele – bez ohledu na metodu pro odkazující tooone na vlastní aplikace. Pomocí **D:\home\site**, jeden přenos sdílených souborů z aplikace tooapp bez nutnosti tooconfigure novou absolutní cestu pro každý přenos.

<a id="TypesOfFileAccess"></a>

### <a name="types-of-file-access-granted-tooan-app"></a>Typy přístupu k souboru udělena tooan aplikace
Každý zákazník předplatné má vyhrazené adresářovou strukturu na sdílenou jednotku UNC konkrétní v rámci datového centra. Zákazník může mít více aplikace vytvořená v rámci konkrétní datové centrum, takže všechny hello adresářů náležejících tooa jednoho zákazníka předplatného se vytvářejí na hello stejné UNC sdílet. sdílené složky Hello může zahrnovat adresáře jsou pro obsah, chyby a diagnostické protokoly a starší verze aplikace hello vytvořené zdrojového kódu. Podle očekávání, adresáře aplikace zákazníka jsou k dispozici pro oprávnění ke čtení a zápisu v době běhu aplikace hello kód aplikace.

Na hello místní jednotky připojené toohello virtuální počítač, který spustí aplikaci, služby App Service si vyhrazuje bloku místa na hello jednotku C:\ pro konkrétní aplikaci dočasné místní úložiště. I když aplikace má čtení/zápisu tooits vlastní dočasný místní úložiště s přístupem, úložiště není skutečně určený toobe používat přímo kód aplikace hello. Místo toho je záměr hello tooprovide ukládání dočasných souborů pro službu IIS a webové aplikace rozhraní. Hello množství dočasné místní úložiště k dispozici tooeach tooprevent jednotlivé aplikace spotřebovat nadměrné množství úložiště místního souboru je také omezení služby App Service.

Jsou dva příklady, jak služba aplikace používá dočasné místní úložiště hello adresář pro dočasné soubory ASP.NET a hello adresář pro službu IIS komprimované soubory. Hello kompilace ASP.NET – systém adresáře "Temporary ASP.NET Files" hello používá jako umístění mezipaměti dočasné kompilace. Služba IIS používá výstup hello "IIS dočasné komprimované soubory" directory toostore komprimované odezvy. Oba tyto typy souborů využití (stejně jako ostatní) jsou přemapování v App Service tooper aplikace dočasné místní úložiště. Přemapování zajistí, že funkce pokračuje podle očekávání.

Každá aplikace ve službě App Service spouští jako identitu náhodných jedinečný nízkými oprávněními pracovního procesu se říká hello "identita fondu aplikací", zde popsané dál: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities ](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Kód aplikace použije tuto identitu pro jednotku operačního systému toohello základní přístup jen pro čtení (hello jednotku D:\). To znamená, že kód aplikace, můžete seznam běžných struktur adresářů a číst společné soubory na jednotce operačního systému. I když to může být nezobrazí toobe poněkud obecné úrovni přístupu, hello stejných adresářích a soubory jsou přístupné při zřizování role pracovního procesu v Azure hostovaná služba a číst obsah jednotky hello. 

<a name="multipleinstances"></a>

### <a name="file-access-across-multiple-instances"></a>Přístup k souborům ve více instancích
Domovský adresář Hello obsahuje obsah aplikace a kód aplikace může zapisovat tooit. Pokud aplikace běží na několik instancí, hello domovský adresář je sdílena mezi všechny instance, tak, aby všechny instance, najdete v části hello stejný adresář. Ano například pokud aplikace uloží odeslané soubory toohello domovský adresář, tyto soubory jsou okamžitě k dispozici tooall instance. 

<a id="NetworkAccess"></a>

## <a name="network-access"></a>Přístup k síti
Kód aplikace můžete použít protokol TCP/IP a UDP, na základě protokolů toomake odchozí síťové připojení tooInternet přístupné koncových bodů, které zveřejňují externích služeb. Aplikace můžete použít tyto stejné protokoly tooconnect tooservices v rámci Azure &#151; například vytvořením tooSQL připojení HTTPS databáze.

Je také omezené funkce pro aplikace tooestablish jeden místní smyčky připojení, a mít aplikaci naslouchání že soketem místní smyčky. Tato funkce existuje především tooenable aplikace, které naslouchat na místní smyčky sockets jako součást jejich funkce. Všimněte si, že každá aplikace uvidí připojení "privátní" zpětné smyčky; aplikace "A" nemůže naslouchat soketu místní smyčky tooa navázat aplikací "B".

Pojmenované kanály jsou podporovány také jako mechanismus meziprocesová komunikace (IPC) mezi různé procesy, které se souhrnně spustí aplikaci. Například modul FastCGI služby IIS hello spoléhá na pojmenované kanály hello toocoordinate jednotlivých se procesy, které spouštějí stránky PHP.

<a id="Code"></a>

## <a name="code-execution-processes-and-memory"></a>Provádění kódu, procesy a paměti
Jak již bylo uvedeno dříve, aplikace běžet uvnitř nízkými oprávněními pracovních procesů pomocí identita fondu náhodných aplikací. Kód aplikace má přístup toohello paměťový prostor přidružené hello pracovní proces, jakož žádné podřízené procesy, které může být vytvořený procesy CGI nebo jiné aplikace. Ale jednu aplikaci nelze získat přístup k paměti hello nebo data i v případě, že je v jiné aplikaci hello stejného virtuálního počítače.

Aplikace můžete spustit skripty nebo napsané pomocí architektury pro vývoj podporované webové stránky. Žádné webové framework nastavení toomore omezený režim neprovede konfiguraci služby App Service. Například ASP.NET aplikace běžící na App Service běžet v "úplná" důvěryhodnosti jako další názvem na rozdíl od tooa omezený režim vztah důvěryhodnosti. Webové rozhraní, včetně classic ASP a ASP.NET, může zavolat komponenty modelu COM v procesu (ale ne mimo proces COM – součásti) jako ADO (ActiveX Data Objects), která jsou zaregistrovaná ve výchozím nastavení operačního systému Windows hello.

Aplikace můžete vytvořit a spustit libovolný kód. Povolených akcí toodo aplikaci je jako spawn příkazové prostředí nebo spustit skript prostředí PowerShell. Nicméně i když může být libovolný kód a procesy vytvořený z aplikace, spustitelné programy a skriptů jsou stále s omezeným přístupem toohello oprávnění uděleno toohello nadřazených fond aplikací. Například může aplikace spawn spustitelné soubory, které umožňuje odchozí volání protokolu HTTP, ale, že stejný spustitelný soubor nelze pokus toounbind hello IP adresu pro virtuální počítač z jeho síťový adaptér. Provádění volání na odchozí sítě je povolený kód toolow oprávněním, ale pokus tooreconfigure nastavení sítě na virtuálním počítači vyžaduje oprávnění správce.

<a id="Diagnostics"></a>

## <a name="diagnostics-logs-and-events"></a>Diagnostické protokoly a události
Informace v protokolu je jinou sadu dat, že některé aplikace se pokusit o tooaccess. typy Hello protokolu informace k dispozici toocode spuštěných ve službě App Service zahrnuje diagnostiky a protokolovat informace generované aplikaci, která je také snadno dostupný toohello aplikace. 

Například protokoly W3C HTTP vygenerovaný active aplikací jsou k dispozici, buď v adresáři protokolu v síti hello sdílet umístění vytvořené pro aplikace hello nebo k dispozici v úložišti objektů blob, pokud zákazník má nastavit toostorage protokolování W3C. druhou možnost Hello umožňuje velké objemy protokoly toobe shromáždit bez hello riziko překročení limitů úložiště souboru hello přidružené k síťové sdílené složky.

V souvislosti podobnou v reálném čase diagnostiky, ke kterému informací z aplikace .NET může přihlášení také pomocí možnosti toowrite trasování a diagnostiku infrastruktury .NET hello hello trasování informace tooeither hello aplikace síťové sdílené složky, případně tooa úložiště objektů blob umístění.

Oblasti diagnostiky protokolování a trasování, které nejsou k dispozici tooapps jsou události Windows ETW a běžné protokoly událostí systému Windows (například systému, aplikace a zabezpečení protokoly událostí). Vzhledem k tomu, že informace o trasování ETW potenciálně lze zobrazit události tooETW oprávnění ke čtení a zápisu celého systému (s hello vpravo seznamy ACL), jsou zablokované. Vývojáři mohou Všimněte si, volání tohoto rozhraní API tooread a zápis události trasování událostí a zobrazí toowork běžné protokoly událostí systému Windows, ale je to způsobeno služby App Service je "faking" hello volání, aby se zobrazily toosucceed. Ve skutečnosti má kód aplikace hello žádná data toothis přístup.

<a id="RegistryAccess"></a>

## <a name="registry-access"></a>Přístup k registru
Aplikace musí mít oprávnění jen pro čtení toomuch (ale ne všechny) hello registru hello virtuálního počítače se systémem. V praxi to znamená, že jsou přístupné pro aplikace klíče registru, které umožňují přístup jen pro čtení toohello místní skupiny uživatelů. Jednou z oblastí hello registru, která není aktuálně podporována pro čtení nebo zápis je hello HKEY\_aktuální\_uživatele hive.

Přístup pro zápis toohello registru je blokovaný, včetně klíče registru pro jednotlivé uživatele tooany přístup. Z hlediska aplikace hello přístup pro zápis toohello registru by nikdy spoléhat v hello prostředí Azure vzhledem k tomu, že aplikace se můžou (a provést) získat proběhne migrace na jiné virtuální počítače. Hello pouze trvalé zapisovatelné úložiště, které může být závisí na aplikaci je hello jednotlivé aplikace adresář s obsahem struktura uložené ve sdílené složky UNC služby aplikace hello. 

## <a name="more-information"></a>Další informace

[Azure izolovaného prostoru webové aplikace](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) -hello většina aktuální informace o prostředí pro spuštění hello služby App Service. Tato stránka se spravuje přímo pomocí hello vývojový tým služby App Service.

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 


