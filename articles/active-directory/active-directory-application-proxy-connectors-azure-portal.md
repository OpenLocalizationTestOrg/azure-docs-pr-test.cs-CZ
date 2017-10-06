---
title: "aaaPublishing aplikací na samostatných sítí a umístění v skupiny konektoru Proxy aplikace Azure AD | Microsoft Docs"
description: "Zahrnuje jak toocreate a spravovat skupiny konektorů v Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: 8c9a84b365eab28eaaeb343d4d1e2e6990537fec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Publikování aplikací na samostatných sítí a umístění pomocí konektoru skupin
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [Portál Azure Classic](active-directory-application-proxy-connectors.md)
>

Zákazníci využívat proxy aplikace služby Azure AD pro více scénářů a aplikace. Proto jsme provedli jsme Proxy aplikace i flexibilnější povolením více topologií. Můžete vytvořit skupiny konektoru Proxy aplikace, takže můžete přiřadit konkrétní konektory tooserve konkrétní aplikace. Tato funkce poskytuje další ovládací prvek a způsoby toooptimize vašemu nasazení proxy serveru aplikace. 

Každý konektor Proxy aplikace je přiřazen tooa konektor skupiny. Všechny hello konektory, které patří toohello stejnou skupinu konektor fungovat jako samostatná jednotka pro vysokou dostupnost a vyrovnávání zatížení. Všechny konektory patří tooa konektor skupiny. Pokud nevytvoříte skupin, všechny konektory jsou ve výchozí skupině. Správce můžete vytvářet nové skupiny a přiřadit toothem konektory v hello portálu Azure. 

Všechny aplikace přiřazené tooa konektor skupiny. Pokud nevytvoříte skupin, vaše aplikace přiřazené tooa výchozí skupiny. Ale pokud je vaše konektory uspořádat do skupin, můžete nastavit každý toowork aplikace se skupinou konkrétní konektor. V takovém případě pouze hello konektory v dané skupině sloužit hello aplikaci na vyžádání. Tato funkce je užitečná, pokud vaše aplikace, které jsou hostované v různých umístěních. Konektor skupiny založené na umístění, můžete vytvořit tak, aby aplikace jsou vždy obsloužených konektory, které jsou fyzicky zavřít toothem.

>[!TIP] 
>Pokud máte velké nasazení služby Proxy aplikace, nepřiřazovat žádné skupiny aplikací toohello výchozí konektor. Tímto způsobem nové konektory nemáte přijímat přenosy za provozu, dokud je nezařadíte tooan konektor služby active skupiny. Tuto konfiguraci můžete také tooput konektory v režimu nečinnosti přesunutím back toohello výchozí skupina, aby mohli provést údržby bez dopadu na vaši uživatelé.

## <a name="prerequisites"></a>Požadavky
toogroup konektory, máte toomake, že jste [nainstalovat více konektorů](active-directory-application-proxy-enable.md). Pokud instalujete nový konektor, co se automaticky připojí hello **výchozí** konektor skupiny.

## <a name="create-connector-groups"></a>Vytvořit konektor skupiny
Pomocí těchto kroků toocreate libovolný počet skupin konektor. 

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
1. Vyberte **Azure Active Directory** > **podnikové aplikace, které** > **proxy aplikace**.
2. Vyberte **nové skupiny konektor**. Otevře se okno nové skupiny konektor Hello.

   ![Vyberte novou skupinu konektoru](./media/active-directory-application-proxy-connectors-azure-portal/new-group.png)

3. Zadejte název nové skupiny konektor, potom použijte tooselect hello rozevírací nabídky, které konektory patří do této skupiny.
4. Vyberte **Uložit**.

## <a name="assign-applications-tooyour-connector-groups"></a>Přiřadit tooyour konektor skupiny aplikací
Tyto kroky použijte pro každou aplikaci, kterou jste publikovali pomocí Proxy aplikace. Při první publikování nebo přiřazení hello toochange tyto kroky můžete použít vždy, když chcete můžete přiřadit skupiny aplikací tooa konektor.   

1. Hello řídicí panel správy pro váš adresář, vyberte **podnikové aplikace, které** > **všechny aplikace** > hello aplikace, které chcete tooassign tooa konektor skupiny >  **Proxy aplikací**.
2. Použití hello **konektor skupiny** rozevírací nabídce tooselect hello skupiny chcete hello toouse aplikace.
3. Vyberte **Uložit** tooapply hello změnu.

## <a name="use-cases-for-connector-groups"></a>Případy použití pro konektor skupiny 

Konektor skupiny jsou užitečné pro různé scénáře, včetně:

### <a name="sites-with-multiple-interconnected-datacenters"></a>Weby s několik datových center vzájemně propojena

Mnoho organizací má číslo vzájemně propojena datových center. V takovém případě chcete tookeep tolik provoz v rámci datového centra hello nejdříve protože datacenter křížové odkazy jsou nákladné a pomalé. Můžete nasadit konektorů v každé datacenter tooserve pouze hello aplikace, které se nacházejí v rámci datového centra hello. Tento postup minimalizuje datacenter křížové odkazy a poskytuje zcela transparentní tooyour uživatele.

### <a name="applications-installed-on-isolated-networks"></a>Aplikace nainstalované v izolovaných sítích

Aplikace může být hostovaný v sítích, které nejsou součástí hello hlavní podnikové síti. V izolovaných sítích tooalso izolovat aplikace toohello síti můžete použít konektory tooinstall vyhrazené skupiny konektor. Obvykle se to stane, když jiného dodavatele udržuje konkrétní aplikace pro vaši organizaci. 

Konektor skupiny povolí tooinstall je vyhrazené konektory pro tyto sítě, které publikovat pouze konkrétní aplikace, díky tomu můžou snadněji a bezpečnější správu aplikací toooutsource toothird dodavatelem.

### <a name="applications-installed-on-iaas"></a>Aplikace nainstalované na IaaS 

Pro aplikace nainstalované v IaaS pro přístup do cloudu konektor skupiny poskytují běžné služby toosecure hello přístup tooall hello aplikace. Konektor skupiny nemáte vytvoření další závislosti na vaší podnikové síti nebo fragmentu hello aplikační prostředí. Konektory lze nainstalovat na každé cloudové datacentrum a sloužit pouze aplikace, které se nacházejí v této síti. Můžete nainstalovat několik konektorů tooachieve vysokou dostupnost.

Trvat jako příklad organizaci, která má několik virtuálních počítačů připojený tootheir vlastní IaaS hostované virtuální sítě. zaměstnanci toouse tooallow tyto aplikace těchto privátních sítí jsou připojené toohello podnikové síti pomocí sítě site-to-site VPN. To umožňuje kvalitní zaměstnanci, kteří se nacházejí na místních. Ale nemusí být ideální pro vzdálení zaměstnanci, protože vyžaduje další místní infrastruktury tooroute přístup, jak můžete vidět v následujícím diagramu hello:

![AzureAD Iaas sítě](./media/application-proxy-publish-apps-separate-networks/application-proxy-iaas-network.png)
  
Pomocí skupin Azure AD Application Proxy connector můžete povolit běžné toosecure hello přístup tooall aplikace služby bez vytvoření další závislosti ve vaší podnikové síti:

![Dodavatelé cloudu Iaas více AzureAD](./media/application-proxy-publish-apps-separate-networks/application-proxy-multiple-cloud-vendors.png)

### <a name="multi-forest--different-connector-groups-for-each-forest"></a>Více doménovými strukturami – konektor jiné skupiny pro každou doménovou strukturu

Většina zákazníků, kteří mají nasazenou Proxy aplikace používají její jednotného přihlašování (SSO) možnosti provedením Kerberos vynuceným delegování použitím (KCD). tooachieve to hello konektor počítače nutné toobe tooa připojené k doméně, která můžete delegovat hello uživatelé směrem k aplikace hello. Použitím KCD podporuje funkce mezi doménovými strukturami. Ale pro společnosti, kteří mají odlišné prostředí více doménovými strukturami bez vztahu důvěryhodnosti mezi nimi, jeden konektor nelze použít pro všechny doménové struktury. 

V takovém případě konkrétní konektory lze nasadit pro každou doménovou strukturu a sadu tooserve aplikace, které byly publikované tooserve hello pouze uživatelé této konkrétní doménové struktury. Každá skupina konektor představuje jiné doménové struktuře. Při hello klienta a většina hello prostředí je jednotná pro všechny doménové struktury, uživatelé se dají přiřadit tootheir doménové struktury aplikací pomocí skupin Azure AD.
 
### <a name="disaster-recovery-sites"></a>Weby pro zotavení po havárii

Existují dva různé přístupy, které můžete provést s lokalitou zotavení po havárii, v závislosti na tom, jak jsou implementované vaše lokality:

* Pokud váš web zotavení po Havárii je integrovaný v režimu aktivní aktivní, kde je úplně stejně jako hlavní web hello a má hello stejné sítě a nastavení služby AD, můžete vytvořit hello konektory na webovém serveru hello zotavení po Havárii v hello stejnou skupinu konektor jako hlavní lokalitu hello. To vám umožňuje Azure AD toodetect převzetí služeb při selhání.
* Pokud váš webový server zotavení po Havárii je oddělené od hlavního webu hello, můžete vytvořit skupinu jiný konektor v lokalitě hello zotavení po Havárii a 1) byl zálohování aplikace nebo 2) ručně přesměrovat hello existující aplikace toohello zotavení po Havárii konektor skupiny podle potřeby.
 
### <a name="serve-multiple-companies-from-a-single-tenant"></a>Sloužit více společností z jednoho klienta

Služby pro více společností související tooimplement a model, ve kterém se u jednoho poskytovatele nasadí a udržuje Azure AD s mnoha různými způsoby. Konektor skupiny pomáhají Dobrý den, správce oddělit hello konektorů a aplikací do různých skupin. Jedním ze způsobů, který je vhodný pro malé firmy, je toohave jednoho klienta Azure AD, zatímco jiné společnosti hello mají své vlastní název domény a sítě. To platí také pro M & A scénáře a situace, kdy jednu oblast IT slouží několik společností charakterem předpisů nebo obchodních důvodů. 

## <a name="sample-configurations"></a>Ukázka konfigurace

Některé příklady, které můžete implementovat, zahrnují hello následujícím konektoru skupiny.
 
### <a name="default-configuration--no-use-for-connector-groups"></a>Výchozí konfigurace – bez použití pro konektor skupiny

Pokud nepoužijete konektor skupiny, bude vaše konfigurace vypadat takto:

![AzureAD žádné skupiny pro konektor](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-1.png)
 
Tato konfigurace je dostatečný pro malá nasazení a testy. Bude také vhodný, pokud má vaše organizace ploché síťové topologie.
 
### <a name="default-configuration-and-an-isolated-network"></a>Výchozí konfigurace a izolovanou síť

Tato konfigurace je vývoj hello výchozí nastavení, ve kterém je konkrétní aplikaci, která běží v izolované síti jako jsou například IaaS virtuální sítě: 

![AzureAD žádné skupiny pro konektor](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-2.png)
 
### <a name="recommended-configuration--several-specific-groups-and-a-default-group-for-idle"></a>Doporučená konfigurace – několik konkrétních skupin a výchozí skupiny pro nečinnosti

Doporučená konfigurace pro komplexní a velké organizace Hello je toohave hello výchozí konektor skupina jako skupinu, která nebude poskytovat všechny aplikace a používá se pro nově nainstalovanou nebo nečinné konektory. Všechny aplikace se zpracovávají pomocí vlastní konektor skupin. To umožňuje všechny hello složitosti scénářů hello popsané výše.

V příkladu hello níže hello společnost má dva datová centra, A a B, s dva konektory, které slouží každou lokalitu. Každá lokalita má jiné aplikace, které běží na něm. 

![AzureAD žádné skupiny pro konektor](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-3.png)
 
## <a name="next-steps"></a>Další kroky

* [Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)
* [Povolení jednoduchého přihlášení](application-proxy-sso-overview.md)


