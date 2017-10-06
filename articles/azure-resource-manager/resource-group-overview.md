---
title: "Přehled služby Správce prostředků aaaAzure | Microsoft Docs"
description: "Popisuje, jak toouse Azure Resource Manageru pro nasazení, správu a řízení přístupu prostředků v Azure."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 76df7de1-1d3b-436e-9b44-e1b3766b3961
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: tomfitz
ms.openlocfilehash: a44fccd96d722c006224145d71cc44292255debf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-overview"></a>Přehled Azure Resource Manageru
Hello infrastrukturu aplikace obvykle tvoří celá řada komponent, může být virtuální počítač, účet úložiště a virtuální síti, nebo webové aplikace, databáze, databázový server a služby 3. stran. Tyto komponenty nevidíte jako samostatné entity, ale jako související a vzájemně provázané části jedné entity. Chcete toodeploy, spravovat a monitorovat jako skupinu. Azure Resource Manager umožňuje toowork s hello prostředky ve vašem řešení jako se skupinou. Můžete nasadit, aktualizovat nebo odstranit všechny hello prostředky pro vaše řešení v rámci jediné koordinované operace. Pro nasazení použijete šablonu a tato šablona může fungovat v různých prostředích, jako například v testovacím, přípravném nebo produkčním prostředí. Resource Manager poskytuje zabezpečení, auditování a označování funkce toohelp spravovat prostředky po nasazení. 

## <a name="terminology"></a>Terminologie
Pokud jste nový tooAzure Resource Manager, nejsou některé podmínky, které nemusí být obeznámeni s.

* **prostředek** - Spravovatelná položka, která je k dispozici prostřednictvím služby Azure. Mezi běžné prostředky patří virtuální počítač, účet úložiště, webová aplikace, databáze nebo virtuální síť, ale existuje i mnoho dalších.
* **skupina prostředků** – Kontejner, který obsahuje související prostředky pro řešení Azure. Skupina prostředků Hello může zahrnovat všechny hello prostředky pro řešení hello nebo jenom prostředky, které chcete toomanage jako skupina. Můžete určit, jak mají prostředky tooallocate tooresource skupin podle díky hello nejvhodnější pro vaši organizaci. Viz [Skupiny prostředků](#resource-groups).
* **Poskytovatel prostředků** -služba poskytující prostředky hello můžete nasadit a spravovat prostřednictvím Resource Manageru. Každý poskytovatel prostředků nabízí operace pro práci s hello prostředky, které jsou nasazeny. Některé běžné zprostředkovatelé prostředků jsou Microsoft.Compute, který poskytuje hello prostředek virtuálního počítače, Microsoft.Storage, který poskytuje prostředků účtu úložiště hello, a Microsoft.Web, který poskytuje prostředky související tooweb aplikace. Viz [Poskytovatelé prostředků](#resource-providers).
* **Šablony Resource Manageru** -soubor A JavaScript Object Notation (JSON), který definuje jeden nebo více prostředků toodeploy tooa skupinu prostředků. Definuje také hello závislosti mezi prostředky hello nasazení. Hello šablony lze použít toodeploy hello prostředky konzistentně a opakovaně. Viz [Nasazení šablon](#template-deployment).
* **deklarativní syntaxe** -syntaxi, která vám umožní stavu "je zde I hodlají toocreate" bez nutnosti toowrite hello posloupnost programování toocreate příkazy ho. šablony Resource Manageru Hello je příkladem deklarativní syntaxe. V souboru hello definujete hello vlastnosti tooAzure toodeploy hello infrastruktury. 

## <a name="hello-benefits-of-using-resource-manager"></a>Hello výhody použití Resource Manager
Resource Manager poskytuje několik výhod:

* Můžete nasadit, spravovat a monitorovat všechny hello prostředky pro vaše řešení jako skupina, nikoli zpracovávat jednotlivě.
* Můžete opakovaně nasadit řešení v celém hello životního cyklu a mít přitom jistotu, že vaše prostředky jsou nasazeny v konzistentním stavu.
* Infrastrukturu můžete spravovat pomocí deklarativních šablon místo skriptů.
* Můžete definovat hello závislosti mezi prostředky, takže se nasadí ve správném pořadí hello.
* Služby tooall řízení přístupu můžete použít ve vaší skupině prostředků, protože do platformy pro správu hello je nativně integrováno řízení přístupu na základě Role (RBAC).
* Můžete použít značky tooresources toologically uspořádat všechny prostředky hello ve vašem předplatném.
* Můžete zpřehlednit fakturaci vaší organizace zobrazením náklady pro skupinu prostředků, sdílení hello stejnou značku.  

Resource Manager poskytuje nový způsob toodeploy a správy vašich řešení. Pokud jste použili hello dřívější model nasazení a chcete toolearn o změnách hello naleznete [nasazení Resource Manager principy a nasazení classic](resource-manager-deployment-model.md).

## <a name="consistent-management-layer"></a>Konzistentní vrstva správy
Resource Manager poskytuje pro hello úlohy, které můžete provést pomocí prostředí Azure PowerShell, rozhraní příkazového řádku Azure, portál Azure, rozhraní REST API a nástroje pro vývoj konzistentní vrstva správy. Všechny nástroje hello použijte sadu běžných operací. Použijete hello nástroje, které pro vás nejvhodnější a můžete je zcela zaměnitelným významem bez nejasnostem. 

Hello následující obrázek ukazuje způsob, jakým všechny nástroje hello interakci s hello stejné rozhraní API Správce prostředků Azure. Hello API předá požadavky služby Správce prostředků toohello, který provádí ověřování a autorizaci požadavků hello. Správce prostředků pak směruje hello požadavky toohello odpovídající prostředek zprostředkovatele.

![Model požadavku Resource Manageru](./media/resource-group-overview/consistent-management-layer.png)

## <a name="guidance"></a>Doprovodné materiály
Hello následující návrhy vám pomohou při práci s vašimi řešeními využít výhod Resource Manager.

1. Definovat a nasazovat infrastruktury prostřednictvím hello deklarativní syntaxi v šablonách Resource Manageru, nikoli imperativní příkazy.
2. Definujte všechny kroky nasazení a konfigurace v šabloně hello. K nastavení svého řešení byste neměli využívat žádné ruční kroky.
3. Spustit imperativní příkazy toomanage vaše prostředky, jako je například toostart nebo zastavení aplikace nebo počítače.
4. Uspořádá prostředky s hello stejný životní cyklus ve skupině prostředků. K ostatnímu uspořádání prostředků využijte značky.

Další doporučení k šablonám najdete v tématu [Osvědčené postupy pro vytváření šablon Azure Resource Manageru](resource-manager-template-best-practices.md).

Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

## <a name="resource-groups"></a>Skupiny prostředků
Při definování skupin prostředků se tooconsider některé důležité faktory:

1. Všechny prostředky hello ve vaší skupině by měly sdílet stejný životní cyklus hello. Nasazujete, aktualizujete a odstraňujete je společně. Pokud některý z prostředků, jako je například databázový server, potřebuje tooexist na mít jiný cyklus nasazení by měla být v jiné skupině prostředků.
2. Každý prostředek může být jenom v jedné skupině prostředků.
3. Můžete přidat nebo odebrat skupinu prostředků tooa prostředků kdykoli.
4. Prostředek můžete přesunout z jedné skupiny tooanother skupiny prostředků. Další informace najdete v tématu [přesunout skupiny prostředků toonew prostředků nebo předplatného](resource-group-move-resources.md).
5. Skupina prostředků může obsahovat prostředky, které se nacházejí v různých oblastech.
6. Skupina prostředků může být použit tooscope řízení přístupu pro akce správy.
7. Prostředek může interagovat s prostředky v dalších skupinách prostředků. Tato interakce je běžné při hello dva prostředky souvisí, ale nesdílejí stejný životní cyklus (například webové aplikace pro připojení databáze tooa) hello.

Při vytváření skupiny prostředků, je třeba tooprovide umístění pro danou skupinu prostředků. Asi vás zajímá, proč skupina prostředků potřebuje umístění. A, pokud hello prostředků může mít jiné umístění než hello skupinu prostředků, proč umístění skupiny prostředků hello vás vůbec?" Skupina prostředků Hello ukládají metadata o hello prostředky. Proto když zadáte umístění pro skupinu prostředků hello, určíte se uloží aby metadata. Pro dodržování předpisů, pravděpodobně bude třeba tooensure data uložená v určité oblasti.

## <a name="resource-providers"></a>Poskytovatelé prostředků
Každý poskytovatel prostředků nabízí sadu prostředků a operací pro práci se službou Azure. Například pokud chcete toostore klíče a tajné klíče, pracujete s hello **Microsoft.KeyVault** poskytovatele prostředků. Tento poskytovatel prostředků nabízí typ prostředku nazvaný **trezory** pro vytvoření trezoru klíčů hello. 

Hello název typu prostředku je ve formátu hello: **{poskytovatele prostředků} / {typ prostředku}**. Je například typ trezoru klíčů hello **Microsoft.KeyVault/vaults**.

Než začnete s nasazením vaše prostředky by měly pomoci porozumět hello dostupných poskytovatelů prostředků. Znalost hello názvy prostředků, zprostředkovatele a prostředky pomáhá definovat prostředky chcete toodeploy tooAzure. Navíc musíte tooknow hello platné umístění a verze rozhraní API pro každý typ prostředku. Další informace najdete v tématu [Zprostředkovatelé a typy prostředků](resource-manager-supported-services.md).

## <a name="template-deployment"></a>Nasazení šablon
S Resource Managerem, můžete vytvořit šablonu (ve formátu JSON), která definuje infrastrukturu hello a konfiguraci řešení Azure. Pomocí šablony můžete řešení opakovaně nasadit v průběhu životního cyklu a mít přitom jistotu, že se prostředky nasadí konzistentně. Když vytvoříte řešení z portálu hello, hello řešení automaticky zahrnovat šablonu nasazení. Nemáte toocreate šablony ze začátku protože můžete začít s hello šablony pro vaše řešení a přizpůsobit toomeet svých konkrétních potřeb. Šablonu pro stávající skupinu prostředků můžete načíst export hello aktuální stav hello skupinu prostředků, nebo zobrazení hello Šablona používaná pro konkrétní nasazení. Zobrazení hello [vyexportované šablony](resource-manager-export-template.md) je vám pomůže blíže toolearn o syntaxi šablony hello.

toolearn o formátu hello hello šablony a jak vytvořit, najdete v části [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md). tooview hello syntaxe JSON pro typy prostředků, najdete v části [definování zdrojů v šablonách Azure Resource Manager](/azure/templates/).

Správce prostředků zpracovává hello šablony jako ostatní žádosti (viz obrázek hello pro [konzistentní vrstva správy](#consistent-management-layer)). Analyzuje hello šablony a převede jeho syntaxe operace REST API pro poskytovatele prostředků odpovídající hello. Například když Resource Manager obdrží šablonu s hello následující definice prostředků:

```json
"resources": [
  {
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "mystorageaccount",
    "location": "westus",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {
    }
  }
]
```

Převede ji toohello definice hello následující operace REST API, která se posílají poskytovatele prostředků Microsoft.Storage toohello:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/mystorageaccount?api-version=2016-01-01
REQUEST BODY
{
  "location": "westus",
  "properties": {
  }
  "sku": {
    "name": "Standard_LRS"
  },   
  "kind": "Storage"
}
```

Jak můžete definovat skupiny prostředků a šablony je zcela až tooyou a jakým způsobem chcete toomanage řešení. Například můžete nasadit aplikaci tři vrstvy prostřednictvím jediné šabloně tooa jedna skupina prostředků.

![třívrstvá šablona](./media/resource-group-overview/3-tier-template.png)

Ale není nutné toodefine celou infrastrukturu v jediné šabloně. Často má smysl toodivide požadavky nasazení do sadu cílových, zaměřené na konkrétní účel šablony. Tyto šablony můžete snadno opakovaně použít pro různá řešení. toodeploy konkrétní řešení, vytvoříte hlavní šablonu, která propojí všechny hello požadované šablony. Hello následující obrázek ukazuje, jak toodeploy tři vrstvy řešení prostřednictvím nadřazené šablonu, která obsahuje tři vnořené šablony.

![šablona vnořených vrstev](./media/resource-group-overview/nested-tiers-template.png)

Pokud vaše vrstev s samostatné životních představ, můžete nasadit vaší skupiny prostředků tooseparate třech úrovních. Všimněte si hello prostředky mohou být stále propojené tooresources v další skupiny zdrojů.

![šablona vrstvy](./media/resource-group-overview/tier-templates.png)

Další rady k navrhování šablon najdete v tématu [Způsoby navrhování šablon Azure Resource Manageru](best-practices-resource-manager-design-templates.md). Informace o vnořených šablonách najdete v tématu [Použití propojených šablon s Azure Resource Managerem](resource-group-linked-templates.md).

Azure Resource Manager analyzuje závislosti, které tooensure prostředky vytvoří ve správném pořadí hello. Pokud jeden prostředek závisí na hodnotě z jiného prostředku (například virtuální počítač potřebuje účet úložiště pro disky), nastavíte závislost. Další informace najdete v tématu [Definování závislostí v šablonách Azure Resource Manageru](resource-group-define-dependencies.md).

Hello šablonu můžete použít také pro aktualizace toohello infrastrukturu. Můžete například přidat řešení tooyour prostředek a konfigurační pravidla pro hello prostředky, které jsou už nasazené. Pokud hello Šablona specifikuje vytvoření prostředku, ale tento prostředek již existuje, Azure Resource Manager provede aktualizaci místo vytvoření nového prostředku. Azure Resource Manager aktualizace hello existující asset toohello stejné stavu, protože by byl nový.  

Pokud potřebujete další operace, jako je instalace konkrétního softwaru, který není zahrnutý v instalačním programu hello Resource Manager poskytuje rozšíření pro scénáře. Pokud již využíváte službu pro správu konfigurace, jako je DSC, Chef nebo Puppet, můžete tuto službu s pomocí rozšíření používat i nadále. Informace o rozšířeních virtuálních počítačů najdete v tématu [Funkce a rozšíření virtuálních počítačů](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Nakonec hello šablony stane součástí hello zdrojového kódu pro vaši aplikaci. Můžete zkontrolovat v tooyour úložiště zdrojového kódu a jej aktualizovat, protože vaše aplikace vyvíjet. Můžete upravit šablonu hello pomocí sady Visual Studio.

Po definování šablony, jsou připravené toodeploy hello prostředky tooAzure. Hello příkazy toodeploy hello zdroje najdete v následujících tématech:

* [Nasazení prostředků pomocí šablon Resource Manageru a Azure PowerShellu](resource-group-template-deploy.md)
* [Nasazení prostředků pomocí šablon Resource Manageru a rozhraní příkazového řádku Azure](resource-group-template-deploy-cli.md)
* [Nasazení prostředků pomocí šablon Resource Manageru a webu Azure Portal](resource-group-template-deploy-portal.md)
* [Nasazení prostředků pomocí šablon Resource Manageru a jeho rozhraní REST API](resource-group-template-deploy-rest.md)

## <a name="tags"></a>Značky
Resource Manager poskytuje funkci označování, která vám umožní toocategorize prostředky podle požadavků tooyour pro správu nebo fakturaci. Použití značek, když máte komplexní kolekci prostředků a jejich skupin a potřebujete toovisualize tyto prostředky hello takovým způsobem, který umožňuje hello většina tooyou smysl. Můžete například označit prostředky, které ve vaší organizaci poskytovat podobnou roli nebo patří toohello stejného oddělení. Bez značky, uživatelé ve vaší organizaci můžete vytvořit různé prostředky, které může být obtížné toolater identifikovat a spravovat. Například můžete toodelete všechny hello prostředky pro konkrétní projekt. Pokud tyto prostředky nejsou označeny pro projekt hello, máte toomanually, kde je najít. Označení může také hrát důležitou roli můžete tooreduce zbytečných nákladů ve vašem předplatném. 

Prostředky nemusí tooreside v hello stejné tooshare skupiny prostředků značku. Můžete vytvořit vlastní značky taxonomii tooensure, že všichni uživatelé ve vaší organizaci používat společné značky spíše než neúmyslně zavádět mírně odlišné značky (třeba odd"místo"oddělení").

Hello následující příklad ukazuje značky použít tooa virtuálního počítače.

```json
"resources": [    
  {
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2015-06-15",
    "name": "SimpleWindowsVM",
    "location": "[resourceGroup().location]",
    "tags": {
        "costCenter": "Finance"
    },
    ...
  }
]
```

použít všechny prostředky hello s hodnotou značky tooretrieve hello následující rutiny prostředí PowerShell:

```powershell
Find-AzureRmResource -TagName costCenter -TagValue Finance
```

Nebo hello následující příkaz Azure CLI 2.0:

```azurecli
az resource list --tag costCenter=Finance
```

Můžete také zobrazit s příznakem prostředkům prostřednictvím hello portálu Azure.

Hello [sestav využití](../billing/billing-understand-your-bill.md) pro vaše předplatné zahrnuje značka názvy a hodnoty, což vám umožní toobreak out náklady podle značky. Další informace o značkách najdete v tématu [pomocí značky tooorganize vašich prostředků Azure](resource-group-using-tags.md).

## <a name="access-control"></a>Řízení přístupu
Resource Manager umožňuje toocontrol, který má přístup toospecific akce pro vaši organizaci. Nativně integruje řízení přístupu na základě role (RBAC) do platformy pro správu hello a použije tento přístup řízení tooall services ve vaší skupině prostředků. 

Existují dvě hlavní koncepty toounderstand při práci s řízení přístupu na základě rolí:

* Definice rolí – popisují sadu oprávnění a lze je použít v mnoha přiřazeních.
* Přiřazení rolí – přidružují definici k identitě (uživatel nebo skupina) určitého oboru (předplatné, skupina prostředků nebo prostředek). nižší obory dědí Hello přiřazení.

Můžete přidat uživatele definované toopre platformy a rolím pro konkrétní prostředky. Můžete například využít výhod hello předdefinovanou roli s názvem čtečka, která umožňuje uživatelům tooview prostředků, ale nemohli je změnit. Přidání uživatelů ve vaší organizaci, kteří potřebují tento typ role Čtenář toohello přístup a použití hello role toohello předplatné, skupinu prostředků nebo prostředek.

Azure poskytuje následující čtyři role platformy hello:

1. Vlastník: Může spravovat vše, včetně přístupu.
2. Přispěvatel: Může spravovat všechno kromě přístupu.
3. Čtenář: Může vše zobrazit, ale nemůže provádět změny.
4. Správce uživatelského přístupu – můžete spravovat prostředky tooAzure přístupu uživatelů

Azure poskytuje také několik rolí specifických pro prostředky. Mezi ty běžné patří:

1. Přispěvatel virtuálních počítačů – můžete spravovat virtuální počítače, ale nejsou udělena přístup toothem a nemůže ani spravovat hello virtuální sítě nebo úložiště účet toowhich, které jsou připojeny
2. Přispěvatel sítě – můžete spravovat všechny síťové prostředky, ale nejsou udělena toothem přístup
3. Přispěvatel účet úložiště – můžete spravovat účty úložiště, ale nejsou udělena toothem přístup
4. Přispěvatel SQL Serveru: Může spravovat servery a databáze SQL, ale ne jejich zásady zabezpečení.
5. Přispěvatel webu – můžete spravovat weby, ale není hello toowhich plány webové, které jsou připojeny

Hello úplný seznam rolí a povolených akcí naleznete v tématu [RBAC: integrovaným rolím](../active-directory/role-based-access-built-in-roles.md). Další informace o řízení přístupu na základě rolí najdete v tématu [Řízení přístupu na základě role v Azure](../active-directory/role-based-access-control-configure.md). 

V některých případech chcete toorun kódu nebo skriptu, který má přístup k prostředkům, ale nechcete, aby toorun ho pověřeními uživatele. Místo toho chcete toocreate identity pro aplikace hello volat hlavní služby a přiřaďte hello vhodnou roli pro hello instanční objekt. Resource Manager umožňuje toocreate přihlašovací údaje pro aplikace hello a ověření prostřednictvím kódu programu hello aplikace. toolearn o vytváření objektů služby, naleznete v následujících tématech:

* [Použití Azure PowerShell toocreate hlavní tooaccess prostředky služby](resource-group-authenticate-service-principal.md)
* [Pomocí rozhraní příkazového řádku Azure toocreate hlavní tooaccess prostředky služby](resource-group-authenticate-service-principal-cli.md)
* [Použít aplikaci portálu toocreate Azure Active Directory a objektu služby, které mají přístup k prostředkům](resource-group-create-service-principal-portal.md)

Je také možné explicitně zamknout důležité prostředky tooprevent uživatele z jejich změně nebo odstranění. Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).

## <a name="activity-logs"></a>Protokoly aktivit
Resource Manager protokoluje všechny operace vedoucí k vytvoření, úpravě nebo odstranění prostředku. Můžete použít hello aktivity protokoly toofind k chybě při odstraňování problémů s nebo toomonitor jak uživatele ve vaší organizaci upravit prostředek. Vyberte protokoly hello toosee, **protokoly aktivity** v hello **nastavení** okno pro skupinu prostředků. Protokoly hello můžete filtrovat podle mnoha různých hodnot, včetně kterou operaci hello inicializované uživatelem. Informace o práci s protokoly aktivity hello najdete v tématu [zobrazení aktivity protokoly toomanage Azure prostředky](resource-group-audit.md).

## <a name="customized-policies"></a>Přizpůsobené zásady
Resource Manager umožňuje přizpůsobit toocreate zásady pro správu vašich prostředků. Hello typy zásad, které vytvoříte, mohou zahrnovat různé scénáře. Můžete u prostředků vynutit dodržování zásad vytváření názvů, omezit, které typy a instance prostředků lze nasadit, nebo omezit, které oblasti mohou hostovat konkrétní typ prostředku. Může vyžadovat hodnota značky na prostředky tooorganize fakturace po odděleních. Můžete vytvořit zásady toohelp snížit náklady a zajistit konzistenci v rámci vašeho předplatného. 

Zásady definujete ve formátu JSON a pak je použijete v celém předplatném nebo u určité skupiny prostředků. Zásady jsou jiné než řízení přístupu na základě rolí, protože jsou použité tooresource typy.

Hello následující příklad ukazuje zásadu, která zajišťuje značka konzistence zadáním, že všechny prostředky zahrnují costCenter značku.

```json
{
  "if": {
    "not" : {
      "field" : "tags",
      "containsKey" : "costCenter"
    }
  },
  "then" : {
    "effect" : "deny"
  }
}
```

Existuje mnoho dalších typů zásad, které lze vytvořit. Další informace najdete v tématu [zásady používání toomanage prostředků a řízení přístupu](resource-manager-policy.md).

## <a name="sdks"></a>Sady SDK
Sady Azure SDK jsou k dispozici pro různé jazyky a platformy. Každá z těchto implementací jazyka je k dispozici prostřednictvím správce balíčků jejího ekosystému a GitHubu.

Zde jsou naše úložiště opensourcových sad SDK. Vítáme zpětnou vazbu, otázky a žádosti o získání dat.

* [Azure SDK pro .NET](https://github.com/Azure/azure-sdk-for-net)
* [Knihovny správy Azure pro Javu](https://github.com/Azure/azure-sdk-for-java)
* [Azure SDK pro Node.js](https://github.com/Azure/azure-sdk-for-node)
* [Azure SDK pro PHP](https://github.com/Azure/azure-sdk-for-php)
* [Azure SDK pro Python](https://github.com/Azure/azure-sdk-for-python)
* [Azure SDK pro Ruby](https://github.com/Azure/azure-sdk-for-ruby)

Informace o používání těchto jazyků s vašimi prostředky najdete na následujících odkazech:

* [Azure pro vývojáře na platformě .NET](/dotnet/azure/?view=azure-dotnet)
* [Azure pro vývojáře v Javě](/java/azure/)
* [Azure pro vývojáře v Node.js](/nodejs/azure/)
* [Azure pro vývojáře v Pythonu](/python/azure/)

> [!NOTE]
> Pokud hello SDK neposkytuje hello požadované funkce, můžete také zavolat toohello [rozhraní REST API Azure](https://docs.microsoft.com/rest/api/resources/) přímo.
> 
> 

## <a name="next-steps"></a>Další kroky
* Jednoduchý úvod tooworking se šablonami, najdete v části [Export šablony Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).
* Podrobnější pokyny k vytvoření šablony najdete v tématu [Vytvoření první šablony Azure Resource Manageru](resource-manager-create-first-template.md).
* Funkce hello toounderstand můžete použít v šabloně, najdete v části [šablony funkcí](resource-group-template-functions.md)
* Informace o použití sady Visual Studio s Resource Managerem najdete v tématu [Vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

Zde je videoukázka tohoto přehledu:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure-Documentation-Shorts/Azure-Resource-Manager-Overview/player]


[powershellref]: https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.2.0/azurerm.resources
