---
title: "AAA Začínáme s Azure Automation | Microsoft Docs"
description: "Tento článek obsahuje přehled služby Azure Automation kontrolou hello návrhu a implementace podrobnosti v hello tooonboard přípravy nabídky z Azure Marketplace."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: 434e8ea28c55ff9bda1d2e46a7a6b8378a3baa0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-automation"></a>Začínáme s Azure Automation

Tato příručka Začínáme představuje základní koncepty související toohello nasazení služby Azure Automation. Pokud jste nový tooAutomation v Azure nebo máte zkušenosti s automatizace pracovního postupu software System Center Orchestrator, tento průvodce vám pomůže pochopit jak tooprepare a zařadit automatizace.  Budou se pak se připravit toobegin vývoj runbooky na podporu vašich automatizačních potřeb procesu. 


## <a name="automation-architecture-overview"></a>Přehled architektury služby Automation

![Přehled Azure Automation](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Služby Azure Automation je software jako služba (SaaS) aplikace, která poskytuje škálovatelného a spolehlivého, víceklientské prostředí tooautomate procesů pomocí runbooků a spravovat tooWindows změny konfigurace a použití konfigurace požadovaného stavu systémy Linux (DSC) v Azure, jiné cloudové služby, nebo místní. Entity obsažené ve vašem účtu Automation, jako jsou runbooky, prostředky a účty Spustit jako, jsou izolované od ostatních účtů Automation v rámci vašeho předplatného a ostatních předplatných.  

Runbooky, které spouštíte v Azure, běží v sandboxech Automation, které jsou hostované ve virtuálních počítačích Azure typu platforma jako služba (PaaS).  Sandboxy Automation poskytují izolaci tenantů pro všechny aspekty spuštění runbooků – moduly, úložiště, paměť, síťové komunikace, datové proudy úlohy atd. Tato role je spravované službou hello a není přístupný z účtu Azure nebo Azure Automation, který jste toocontrol.         

tooautomate hello nasazení a správu prostředků ve vašem místním datovém centru nebo jiných cloudových služeb, po vytvoření účtu Automation, můžete určit jeden nebo více počítačů toorun hello [Hybrid Runbook Worker (HRW)](automation-hybrid-runbook-worker.md) role.  Každý HRW vyžaduje hello Agent pro správu Microsoft pomocí pracovního prostoru analýzy protokolů tooa připojení a účet Automation.  Analýzy protokolů je použité toobootstrap hello instalaci, údržbě hello Microsoft Management agenta a monitorování funkce hello hello HRW.  Hello doručování sady runbook a hello toorun instrukce, které je provádí Azure Automation.

Můžete nasadit více HRW tooprovide vysoké dostupnosti pro své sady runbook, úlohy sady runbook Vyrovnávání zatížení a v některých případech je vyhradit pro konkrétní úlohy nebo prostředí.  Hello agenta Microsoft Monitoring Agent na hello HRW inicializuje komunikaci s hello služby Automation přes TCP port 443 a neexistují žádné brány firewall pro příchozí požadavky.  Jakmile máte runbook systémem HRW prostředí hello a má hello runbook tooperform úlohy správy na jiné počítače nebo služby v rámci prostředí, může být jiné porty, které hello runbook potřebuje přístup k.  Pokud vaše zásady zabezpečení IT neumožňují počítačů ve vaší síti tooconnect toohello Internet, přečtěte si článek hello [OMS brány](../log-analytics/log-analytics-oms-gateway.md), které slouží jako proxy pro hello HRW toocollect stav úlohy a přijímat informace o konfiguraci z váš účet Automation.

Runbooky, které běží na HRW spustit v kontextu hello hello místní systémový účet v počítači hello, který se hello doporučuje kontext zabezpečení při provádění akce správy v místním počítači Windows hello. Pokud chcete hello runbook toorun úlohy s prostředky mimo hello místního počítače, musíte toodefine prostředků zabezpečení přihlašovacích údajů v hello účet Automation, můžete přístup z hello runbooku a tooauthenticate pomocí externí prostředek hello. Můžete použít [pověření](automation-credentials.md), [certifikát](automation-certificates.md), a [připojení](automation-connections.md) prostředky ve vašem runbooku pomocí rutin, které umožní toospecify pověření tak je, můžete ověřovat.

Konfigurace DSC uložené ve službě Azure Automation může být přímo použité tooAzure virtuálních počítačů. Další fyzické a virtuální počítače může požádat o konfigurace z načítacího serveru Azure Automation DSC hello.  Pro správu konfigurace vaší místní fyzické nebo virtuální systémy Windows a Linux, není nutné toodeploy všechny infrastruktury toosupport hello server Automation DSC za, jenom odchozí přístup k Internetu z každého systému toobe spravuje Automation DSC , komunikaci přes TCP port 443 toohello OMS služby.   

## <a name="prerequisites"></a>Požadavky

### <a name="automation-dsc"></a>Automatizace DSC
Azure Automation DSC lze použít toomanage různé počítače:

* Virtuální počítače Azure (klasické) s Windows nebo Linuxem
* Virtuální počítače Azure s Windows nebo Linuxem
* Virtuální počítače AWS (Amazon Web Services) s Windows nebo Linuxem
* Fyzické nebo virtuální počítače s Windows v místním prostředí nebo v jiném cloudu než Azure nebo AWS
* Fyzické nebo virtuální počítače s Linuxem v místním prostředí nebo v jiném cloudu než Azure nebo AWS

pro agenta hello DSC prostředí PowerShell pro Windows toobe možné toocommunicate u automatizace Azure, musí být nainstalováno Hello nejnovější verze WMF 5. nejnovější verzi hello Hello [DSC Powershellu agenta pro Linux](https://www.microsoft.com/en-us/download/details.aspx?id=49150) pro Linux toobe možné toocommunicate u automatizace Azure, musí být nainstalováno.

### <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker  
Při určování runbook počítače toorun hybridní úlohy, v tomto počítači musí být hello následující:

* Windows Server 2012 nebo novější
* Windows PowerShell 4.0 nebo novější  Doporučujeme, abyste instalaci prostředí Windows PowerShell 5.0 hello počítače, pro větší spolehlivost. Hello novou verzi si můžete stáhnout z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=50395)
* Minimálně dvě jádra
* Minimálně 4 GB paměti RAM

### <a name="permissions-required-toocreate-automation-account"></a>Oprávnění požadované toocreate účet Automation.
toocreate nebo aktualizace účtu Automation, musí mít hello následující konkrétní oprávnění a vyžadují oprávnění toocomplete v tomto tématu.   
 
* V pořadí toocreate účet Automation, účtu uživatele AD musí toobe přidané tooa role s roli vlastníka ekvivalentní toohello oprávnění pro Microsoft.Automation prostředky podle pokynů v článku [řízení přístupu na základě Role ve službě Azure Automation ](automation-role-based-access-control.md).  
* Pokud registrace aplikace hello nastavení je nastaven příliš**Ano**, mohou uživatelé bez oprávnění správce v klientovi služby Azure AD [registraci aplikací AD](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions).  Pokud registrace aplikace hello nastavení je nastaven příliš**ne**, uživatel hello provedení této akce musí být globálním správcem ve službě Azure AD. 

Pokud si nejste členem instanci Active Directory hello předplatné, předtím, než jsou přidány toohello globální správce nebo řidičská-administrator role hello předplatné, se přidají tooActive Directory jako Host. V takovém případě se zobrazí "Nemáte oprávnění toocreate..." upozornění na hello **přidat účet Automation** okno. Uživatelé, kteří byly přidány toohello globální správce nebo řidičská-administrator role nejprve lze odebrat z instance služby Active Directory hello předplatného a znova se přidal toomake je úplné uživatelské ve službě Active Directory. tooverify této situaci se z hello **Azure Active Directory** podokně hello portál Azure, vyberte **uživatelů a skupin**, vyberte **všichni uživatelé** a po výběru hello konkrétního uživatele, vyberte **profil**. Hello hodnotu hello **typ uživatele** atribut v profilu uživatele hello nesmí rovnat **hosta**.

## <a name="authentication-planning"></a>Plánování ověřování
Automatizace Azure umožňuje tooautomate úlohy s prostředky v Azure, místně a u jiných poskytovatelů cloudu.  Aby runbook tooperform požadované akce, musí mít oprávnění toosecurely přístup k hello prostředkům s minimálními požadovanými v rámci předplatného hello právy hello.  

### <a name="what-is-an-automation-account"></a>Co je účet Automation 
Všechny úlohy hello automation, který děláte s prostředky pomocí hello rutin Azure ve službě Azure Automation ověření tooAzure použití ověřování na základě přihlašovacích údajů organizační identity Azure Active Directory.  Účet automatizace je oddělené od hello účet používáte toosign v portálu tooconfigure toohello a použijte prostředků Azure.  Součástí účtu prostředky služby Automation jsou hello následující:

* **Certifikáty** – obsahuje certifikát používaný k ověřování z runbooku nebo konfigurace DSC nebo je možné ho přidat.
* **Připojení** – obsahuje ověřování a konfigurace požadované informace tooconnect tooan externí služby nebo aplikace ze sady runbook nebo konfigurace DSC.
* **Přihlašovací údaje** -je objekt PSCredential, který obsahuje zabezpečovací přihlašovací údaje, jako je například uživatelské jméno a heslo požadované tooauthenticate ze sady runbook nebo konfigurace DSC.
* **Integrační moduly** -jsou moduly Powershellu, které jsou součástí Azure Automation účet toomake použití rutin v runboocích a konfiguracích DSC.
* **Plány** – obsahuje plány, které v zadaném čase spouští nebo zastavují runbooky, včetně frekvencí opakování.
* **Proměnné** – obsahuje hodnoty, které jsou dostupné z runbooku nebo konfigurace DSC.
* **Konfigurace DSC** -jsou skripty prostředí PowerShell, které popisuje, jak tooconfigure funkce operačního systému nebo nastavení, nebo nainstalovat aplikace na počítači s Windows nebo Linux.  
* **Runbooky** – jsou sady úloh, které provádějí určité automatizované procesy v Azure Automation na základě Windows PowerShellu.    

Hello prostředky Automation jednotlivých účtů Automation jsou přidružené k jedné oblasti Azure, ale účty Automation můžou spravovat všechny prostředky hello ve vašem předplatném. Vytvoření účtů Automation v různých oblastech, pokud máte zásady, které vyžadují datům a prostředkům toobe izolované tooa určité oblasti.

> [!NOTE]
> Účty Automation a hello prostředky, které obsahují jsou vytvořené v hello portálu Azure, nelze získat přístup v hello portál Azure classic. Pokud chcete toomanage tyto účty nebo jejich prostředky pomocí prostředí Windows PowerShell, je nutné použít hello moduly Azure Resource Manageru.
> 

Když vytvoříte účet služby Automation v hello portálu Azure, můžete vytvořit automaticky dvě entity ověřování:

* Účet Spustit jako. Tento účet vytvoří instanční objekt ve službě Azure Active Directory (Azure AD) a certifikát. Také přiřadí hello Přispěvatel přístupu na základě role řízení (RBAC), která spravuje prostředky Resource Manageru pomocí sad runbook.
* Účet Spustit jako pro Azure Classic. Tento účet odešle certifikát správy, což je použité toomanage klasické prostředky pomocí sad runbook.

Řízení přístupu na základě role je k dispozici s Azure Resource Manager toogrant povolené akce tooan Azure AD uživatelský účet a účet Spustit jako a ověřování takového objektu služby.  Čtení [řízení přístupu na základě Role v Azure Automation článku](automation-role-based-access-control.md) pro další informace o toohelp vývojem vašeho modelu pro správu oprávnění automatizace.  

#### <a name="authentication-methods"></a>Metody ověřování
Hello následující tabulka shrnuje hello různé metody ověřování pro jednotlivá prostředí podporovaná službou Azure Automation.

| Metoda | Prostředí 
| --- | --- | 
| Účet Spustit jako pro Azure a Spustit jako pro Azure Classic |Azure Resource Manager a nasazení Azure Classic |  
| Uživatelský účet Azure AD |Azure Resource Manager a nasazení Azure Classic |  
| Ověřování systému Windows |Místní datové centrum nebo jiného poskytovatele cloudu pomocí hello hybridní pracovní proces Runbooku |  
| Přihlašovací údaje služby Amazon Web Services |Amazon Web Services |  

V části hello **jak to\Authentication a zabezpečení** část, jsou podporu články poskytuje přehled a doporučený postup tooconfigure ověřování pro tato prostředí, buď pomocí existující nebo nové účet vyhradit pro prostředí.  Hello spustit v Azure jako a účet Classic spustit jako, hello tématu [aktualizace účtu Automation spustit jako](automation-create-runas-account.md) popisuje, jak tooupdate existujícího účtu automatizace s hello spustit jako účty z hello portálu nebo pomocí prostředí PowerShell, pokud nebyl původně nakonfigurovaný s účtem spustit jako nebo Classic spustit jako. Pokud chcete toocreate spustit jako a účet Classic spustit jako s certifikát vydaný certifikační autoritou (CA) rozlehlé sítě, přečtěte si tento článek toolearn jak účtů toocreate hello pomocí této konfigurace.     
 
## <a name="network-planning"></a>Plánování sítě
Pro hello hybridní pracovní proces Runbooku tooconnect tooand registrace s Microsoft Operations Management Suite (OMS) musí mít číslo portu toohello přístupu a adresy URL hello popsané dole.  Toto je navíc toohello [portů a adres URL potřebných pro agenta Microsoft Monitoring Agent hello](../log-analytics/log-analytics-windows-agents.md#network) tooconnect tooOMS. Pokud používáte proxy server pro komunikaci mezi hello agenta a hello OMS služby, je třeba tooensure, jestli jsou dostupné odpovídající prostředky hello. Pokud používáte bránu firewall toorestrict přístup toohello Internet, musíte tooconfigure přístup toopermit brány firewall.

Následující seznam hello portů a adres URL, které jsou požadovány pro hello hybridní pracovní proces Runbooku toocommunicate s automatizace Hello informace.

* Port: Vyžaduje se jenom TCP 443 pro odchozí internetový přístup.
* Globální adresa URL: *.azure-automation.net

Pokud máte účet Automation definované pro konkrétní oblasti a chcete toorestrict komunikaci s místní datového centra, hello následující tabulka obsahuje hello DNS záznam pro každou oblast.

| **Oblast** | **Záznam DNS** |
| --- | --- |
| Střed USA – jih |scus-jobruntimedata-prod-su1.azure-automation.net |
| Východní USA 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Západní střed USA | wcus-jobruntimedata-prod-su1.azure-automation.net |
| Západní Evropa |we-jobruntimedata-prod-su1.azure-automation.net |
| Severní Evropa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Střední Kanada |cc-jobruntimedata-prod-su1.azure-automation.net |
| Jihovýchodní Asie |sea-jobruntimedata-prod-su1.azure-automation.net |
| Střed Indie |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japonsko – východ |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Austrálie – jihovýchod |ase-jobruntimedata-prod-su1.azure-automation.net |
| Spojené království – jih | uks-jobruntimedata-prod-su1.azure-automation.net |
| USA (Gov) – Virginia | usge-jobruntimedata-prod-su1.azure-automation.us |

Seznam IP adres namísto názvů, stáhnout a revidovat hello [Azure Datacenter IP adresu](https://www.microsoft.com/download/details.aspx?id=41653) souboru xml z hello Microsoft Download Center. 

> [!NOTE]
> Tento soubor obsahuje hello rozsahy IP adres (včetně rozsahů výpočty, SQL a úložiště) používané v hello datová centra služby Microsoft Azure. Aktualizovaný soubor je odeslat každý týden, což odráží hello aktuálně nasazená rozsahů a rozsahů IP toohello žádné nadcházející změny. Nové rozsahy, které jsou uvedeny v souboru hello nebudou použity v datových centrech hello alespoň jeden týden. Prosím nové xml hello stažení souboru každý týden a provést potřebné změny hello na svém webu toocorrectly identifikovat služby spuštěné v Azure. Expresní trasy uživatelé mohou Poznámka: Tento soubor použít tooupdate hello inzerování protokolu BGP Azure místa v hello první týden v měsíci. 
> 

## <a name="creating-an-automation-account"></a>Vytvoření účtu Automation

Existují různé způsoby, můžete vytvořit účet Automation v hello portálu Azure.  Hello následující tabulka uvádí každý typ nasazení a rozdíly mezi nimi.  

|Metoda | Popis |
|-------|-------------|
| Vyberte ovládací prvek a automatizace hello Marketplace. | Nabídky, která vytvoří účtu Automation a pracovním prostorem OMS propojené tooone druhou v hello stejné skupiny prostředků a oblast.  Integrace s OMS také zahrnuje hello výhodou používání toomonitor analýzy protokolů a analyzovat runbook úlohy stav úlohy datové proudy a v čase a využívat pokročilé funkce tooescalate nebo prozkoumat problémy. Hello nabídka také nasadí sledování změn a Správa aktualizací hello řešení, které jsou ve výchozím nastavení povolené. |
| Vyberte automatizace hello Marketplace. | Vytvoří účet Automation ve skupině nový nebo existující prostředek, který není pracovním prostorem OMS propojené tooan a neobsahuje žádné dostupné řešení z nabídky hello automatizace a řízení. Toto je základní konfigurace, která vás seznámí tooAutomation a pomáhají vám pochopit, jak toowrite sady runbook, konfigurace DSC a použití hello možnosti služby hello. |
| Vybraná řešení pro správu | Pokud vyberete řešení –  **[správy aktualizací](../operations-management-suite/oms-solution-update-management.md)**,  **[spuštění a zastavení virtuálních počítačů během mimo špičku](automation-solution-vm-management.md)**, nebo  **[ Sledování změn](../log-analytics/log-analytics-change-tracking.md)**  vyzvat vás tooselect existující automatizace a pracovním prostorem OMS nebo nabízejí hello možnost toocreate jako požadovány pro toobe hello řešení nasazené v rámci vašeho předplatného. |

Toto téma vás provede procesem vytváření účet Automation a pracovním prostorem OMS registrace hello automatizace a řízení nabídky.  toocreate samostatný účet Automation pro testování nebo toopreview hello službu, zkontrolujte hello následujícího článku [vytvořit účet Automation samostatné](automation-create-standalone-account.md).  

### <a name="create-automation-account-integrated-with-oms"></a>Vytvoření účtu Automation integrovaného s OMS
Hello doporučená metoda tooonboard, automatizace se tak, že vyberete hello automatizace & ovládacího prvku nabídka z hello Marketplace.  Tím se vytvoří účet Automation a vytváří hello integrace s pracovním prostorem OMS, včetně hello možnost tooinstall hello správu řešení, které jsou k dispozici s hello nabídky.  

1. Přihlaste se toohello portálu Azure pomocí účtu, který je členem role Správci předplatného hello a spolusprávce předplatného hello.

2. Klikněte na možnost **Nové**.<br><br> ![Výběr možnosti Nové na webu Azure Portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  

3. Vyhledejte **automatizace** a pak v hello výsledky hledání vyberte **automatizace a řízení***.<br><br> ![Hledání a výběr Automation &amp; Control na Marketplace](media/automation-offering-get-started/automation-portal-martketplace-select-automationandcontrol.png)<br>   

4. Po přečtení hello popis hello nabídky, klikněte na tlačítko **vytvořit**.  

5. Na hello **automatizace a řízení** okno nastavení, vyberte **pracovním prostorem OMS**.  Na hello **pracovních prostorů OMS** okně vyberte toohello propojené pracovní prostor OMS stejného předplatného Azure, který hello účet Automation je v nebo vytvořte pracovní prostor služby OMS.  Pokud nemáte pracovním prostorem OMS, vyberte **vytvořit nový pracovní prostor** a na hello **pracovním prostorem OMS** okno provést hello následující: 
   - Zadejte název nové hello **pracovním prostorem OMS**.
   - Vyberte **předplatné** toolink tooby výběr z rozevíracího seznamu hello Pokud hello výchozí vybraný není vhodná.
   - U položky **Skupina prostředků** můžete vytvořit skupinu prostředků nebo vybrat už existující skupinu prostředků.  
   - Vyberte **Umístění**.  V současné době jsou k dispozici pouze umístění hello **Austrálie – jihovýchod**, **východní USA**, **jihovýchodní Asie**, **– Západ střední USA**a  **Západní Evropa**.
   - Vyberte možnost u položky **Cenová úroveň**.  Hello řešení je k dispozici v dvou vrstev: uvolněte a vrstvy na uzel (OMS).  úroveň free Hello může mít na hello množství dat shromážděných denně, doba uchování dat a minut runtime úlohy sady runbook.  úroveň Hello na uzel (OMS) nemá na hello množství dat denně shromážděných omezení.  
   - Vyberte **Účet Automation**.  Pokud vytváříte nový pracovní prostor OMS, je nutné tooalso vytvořit účet Automation, který je spojena s hello nový pracovní prostor OMS dříve, zadaný včetně vašeho předplatného Azure, skupinu prostředků a oblast.  Můžete vybrat **vytvořit účet Automation** a na hello **účet Automation** okno, zadejte následující hello: 
  - V hello **název** pole, zadejte název hello hello účet Automation.

    Všechny ostatní možnosti se vyplní automaticky v závislosti na vybrané pracovní prostor OMS hello a tyto možnosti nelze změnit.  Účet spustit v Azure jako je metoda ověřování výchozí hello hello nabídky.  Po kliknutí na tlačítko **OK**, možnosti konfigurace hello se ověří a vytvoření hello účet Automation.  V části jeho průběh můžete sledovat **oznámení** nabídce hello. 

    Další možností je vybrat existující účet služby Automation Spustit jako.  Hello účet, který jste vybrali již nemůže být pracovní prostor OMS propojené tooanother, v opačném případě se zobrazí oznámení v okně hello.  Pokud už je propojená, třeba tooselect jiný účet Automation spustit jako nebo vytvořit.

    Po dokončení hello požadované informace, klikněte na tlačítko **vytvořit**.  ověření Hello informace a vytvoření účtů hello účet Automation a spustit jako.  Jste vráceni toohello **pracovním prostorem OMS** okno automaticky.  

6. Po zadání hello požadované informace na hello **pracovním prostorem OMS** okně klikněte na tlačítko **vytvořit**.  Při ověření hello informace a při vytváření pracovního prostoru hello, můžete sledovat průběh v části **oznámení** nabídce hello.  Jste vráceni toohello **přidat řešení** okno.  

7. Na hello **automatizace a řízení** okno nastavení, potvrďte chcete tooinstall hello doporučená předem vybraného řešení. Pokud výběr některého z nich zrušíte, můžete ho nainstalovat později.  

8. Klikněte na tlačítko **vytvořit** tooproceed s registrací ve službě Automation a pracovním prostorem OMS. Všechna nastavení se ověří a následně se ho pokusí toodeploy hello nabídky v rámci vašeho předplatného.  Tento proces může trvat několik sekund toocomplete a vy můžete sledovat v jeho průběhu **oznámení** nabídce hello. 

Po hello nabídka je zařazený nemá, můžete začít vytvářet sady runbook, pracují s hello jste povolili řešení pro správu, nasazení [hybridní pracovní proces Runbooku](automation-hybrid-runbook-worker.md) role nebo začít pracovat s [analýzy protokolů](https://docs.microsoft.com/azure/log-analytics) toocollect data generována prostředků ve vašem prostředí cloudu nebo místně.   

## <a name="next-steps"></a>Další kroky
* Pokud chcete ověřit, že nový účet Automation umožňuje ověřování prostřednictvím prostředků Azure, prohlédněte si [test ověřování účtu Azure Automation Spustit jako](automation-verify-runas-authentication.md).
* tooget začít s vytvářením sad runbook, přečtěte si nejprve hello [typy runbooků Automation](automation-runbook-types.md) podporována a související informace před zahájením vytváření.


