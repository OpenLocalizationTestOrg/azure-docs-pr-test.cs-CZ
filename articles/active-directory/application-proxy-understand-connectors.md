---
title: aaaUnderstand Azure AD Application Proxy konektory | Microsoft Docs
description: "Popisuje hello základní informace o Azure AD Application Proxy konektory."
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
ms.date: 08/03/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 294cb26803ef7cf8be9f3af0678d6d2e64f6cc14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-ad-application-proxy-connectors"></a>Pochopení konektory proxy aplikace služby Azure AD

Konektory jsou co umožňují proxy aplikace služby Azure AD. Je jednoduchá, snadno toodeploy a udržovat a superuživatele výkonné. Tento článek popisuje, jaké konektory jsou, jak fungují a některé návrhy, jak toooptimize vaše nasazení. 

## <a name="what-is-an-application-proxy-connector"></a>Co je konektor Proxy aplikace

Konektory jsou lightweight agenti, kteří se nacházejí na místě a usnadnit tak hello odchozí připojení toohello Proxy aplikace služby. Konektory musí být nainstalován v systému Windows Server, který má přístup toohello back-end aplikace. Konektory můžete uspořádat do skupin konektoru se všemi skupinami zpracování provoz toospecific aplikace. Vyrovnávání zatížení konektory automaticky a může pomoct toooptimize struktury vaší sítě. 

## <a name="requirements-and-deployment"></a>Požadavky a nasazení

toodeploy Proxy aplikace úspěšně, budete potřebovat minimálně jeden konektor, ale doporučujeme dvou nebo více větší odolnost. Hello konektor nainstalujte na počítač 2016 nebo Windows Server 2012 R2. Hello konektor musí mít toocommunicate toobe s hello Proxy aplikace služby a také hello místní aplikace, které můžete publikovat. 

Další informace o hello síťové požadavky u hello serveru konektoru najdete v tématu [začít pracovat s Proxy aplikace a nainstalujte konektor](active-directory-application-proxy-enable.md).

## <a name="maintenance"></a>Údržby
Hello konektory a hello služby postará o všechny úlohy hello vysokou dostupnost. Dají se přidat nebo odebrat dynamicky. Pokaždé, když dorazí novou žádost o jedná směrované tooone hello konektorů, které jsou aktuálně dostupné. Pokud konektor není dočasně k dispozici, nereaguje toothis provoz.

Hello konektory jsou bezstavové a mít žádné konfigurační data v počítači hello. Hello pouze data, která ukládají se nastavení hello připojení hello service a její ověřovací certifikát. Při připojování toohello služby, všechna data hello požadované konfigurace pro vyžádání obsahu a jej aktualizovat každých několik minut.

Konektory také dotazování toofind server hello se, zda existuje novější verze konektoru hello. Pokud ho najde, konektory hello aktualizovat sami.

Vaše konektory z hello počítači, na kterém běží, můžete monitorovat pomocí hello protokolu událostí a čítače výkonu. Nebo můžete zobrazit jejich stav ze stránky Proxy aplikace hello hello portálu Azure:

 ![Konektory Proxy aplikace AzureAD](./media/application-proxy-understand-connectors/app-proxy-connectors.png)

Nemáte toomanually odstranit konektory, které se nepoužívá. Konektor je spuštěna, zůstává aktivní jako připojení toohello služby. Nepoužívané konektory jsou označené jako _neaktivní_ a jsou odebrány po 10 dnů nečinnosti. Pokud chcete toouninstall konektor, ale hello službu konektoru i aktualizační službu hello z odinstalujte hello server. Restartujte služby počítače odebrat toofully hello.

## <a name="automatic-updates"></a>Automatické aktualizace

Azure AD poskytuje funkce Automatické aktualizace pro všechny konektory hello, které nasadíte. Tak dlouho, dokud hello Application Proxy Connector Updater service běží, vaše konektory aktualizovat automaticky. Pokud nevidíte hello Connector Updater službu na serveru, musíte příliš[přeinstalování vašeho konektoru](active-directory-application-proxy-enable.md) tooget žádné aktualizace. 

Pokud nechcete, aby toowait pro konektor tooyour toocome automatických aktualizací, můžete provést ruční upgrade. Přejděte toohello [stránky pro stažení konektoru](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download) na hello serveru, kde je vaše konektor najít a vyberte **Stáhnout**. Tento proces se spustí upgrade pro místní konektor hello. 

U klientů s více konektorů automatické aktualizace hello cíle jeden konektor v daný okamžik v každé skupiny tooprevent výpadek ve vašem prostředí. 

Výpadek se můžete setkat, když vaše konektor aktualizuje, pokud:  
- Můžete mít pouze jeden konektor. tooavoid tento výpadek a zvýšit vysokou dostupnost, doporučujeme nainstalovat druhý konektor a [vytvořte skupinu konektor](active-directory-application-proxy-connectors-azure-portal.md).  
- Konektor byla hello středu transakce, když zahájil hello aktualizace. I když dojde ke ztrátě hello počáteční transakce, by měl váš prohlížeč hello operaci automaticky opakovat, nebo můžete obnovit stránku. Když je nutno hello požadavku, hello přenosy jsou směrované tooa zálohování konektor.

## <a name="creating-connector-groups"></a>Vytváření skupin konektoru

Konektor skupiny umožňují tooassign konkrétní konektory tooserve konkrétní aplikace. Můžete seskupit několik konektorů a pak mu přiřaďte každou tooa skupinu aplikací. 

Konektor skupiny umožňují snadnější toomanage nasazení ve velkých organizacích. Také zvyšují latence pro klienty, kteří mají aplikace hostované v různých oblastech, protože můžete vytvořit skupiny založené na umístění konektoru tooserve pouze místní aplikace. 

toolearn Další informace o konektoru skupiny, najdete v části [publikování aplikací na samostatných sítí a umístění pomocí konektoru skupiny](active-directory-application-proxy-connectors-azure-portal.md).

## <a name="security-and-networking"></a>Zabezpečení a sítě

Konektory lze nainstalovat kdekoli v síti hello, která umožňuje jejich toosend požadavky toohello Proxy aplikace služby. Co je důležité je, že tento počítač hello spuštěným konektorem hello také má přístup tooyour aplikace. Konektory můžete nainstalovat v rámci vaší podnikové síti nebo na virtuální počítač, který běží v cloudu hello. Konektory lze spustit v rámci demilitarizovaná zóna (DMZ), ale není nutné, protože všechny přenosy je odchozí, takže zůstává zabezpečené sítě.

Konektory odeslat pouze odchozí požadavky. Hello odchozí přenosy se budou odesílat toohello Proxy aplikace služby a toohello publikovaným aplikacím. Nemáte tooopen příchozí porty, protože přenosy obou směrech po vytvoření relace. Nemáte tooset až vyrovnávání zátěže mezi hello konektory nebo nakonfigurujte příchozí přístup přes vaší brány firewall. 

Další informace o konfiguraci odchozí pravidla brány firewall najdete v tématu [pracují se stávající místní proxy servery](application-proxy-working-with-proxy-servers.md).

Použití hello [Azure AD Application proxy serveru konektoru porty nástroj pro testování](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify, aby vaše konektor přístup hello Proxy aplikace služby. Minimálně zkontrolujte, že oblast hello střed USA a tooyou nejbližší oblast hello jsou všechny zelené značky zaškrtnutí. Kromě toho další zelené značky zaškrtnutí znamená větší odolnost proti chybám. 

## <a name="performance-and-scalability"></a>Výkon a škálovatelnost

Měřítko pro hello Proxy aplikace služby je transparentní, ale Škálováním je faktor pro konektory. Je nutné toohave dostatek konektory toohandle nejsilnějšího provozu. Ale nepotřebujete, Vyrovnávání zatížení tooconfigure, protože všechny konektory v rámci skupiny pro konektor automatické vyvážení zatížení.

Vzhledem k tomu, že konektory jsou bezstavové, nejsou ovlivněny hello počet uživatelů a relací. Místo toho budou odpovídat toohello počet požadavků a jejich velikost datové části. S standardní webových přenosů průměrná počítače může zpracovávat několik tisíců požadavků za sekundu. konkrétní kapacitu Hello závisí na vlastnosti přesný počítač hello. 

výkon konektoru Hello je svázaná s procesoru a sítě. Výkon procesorů je potřeba pro SSL šifrování a dešifrování, zatímco sítě je důležité tooget rychlého připojení toohello aplikace a online služby hello v Azure.

Naproti tomu paměti je menší problém pro konektory. Hello online služba má na starosti mnohem hello zpracování a veškerý provoz neověřené. Vše, co můžete udělat v cloudu hello se provádí v cloudu hello. 

Dalším faktorem, který ovlivňuje výkon je hello kvalitu hello sítě mezi hello konektorů, včetně: 

* **Hello online služby**: toohello pomalá nebo vysokou latencí připojení služby Proxy aplikace v Azure vliv výkon konektoru hello. Pro zajištění nejlepšího výkonu hello připojení Express Route tooAzure vaší organizace. Jinak máte tým sítí, ujistěte se, že tooAzure připojení jsou zpracovávány efektivní chod. 
* **Hello back-end aplikace**: V některých případech jsou další servery proxy mezi hello konektoru a hello back-end aplikace, které můžou způsobit snížení nebo znemožní připojení. tootroubleshoot tento scénář z hello serveru konektoru otevřete prohlížeč a zkuste to tooaccess hello aplikace. Pokud spustíte hello konektory v Azure, ale hello aplikace jsou na místě, nemusí být hello prostředí očekávat vaši uživatelé.
* **Hello řadiče domény**: Pokud hello konektory provést jednotné přihlašování pomocí omezené delegování Kerberos, kontaktujte řadiče domény hello před odesláním hello požadavek toohello back-end. konektory Hello mají mezipaměť lístky protokolu Kerberos, ale v prostředí zaneprázdněn hello odezvy hello řadičů domény může ovlivnit výkon. Tento problém je dnes běžné pro konektory, které spustit v Azure, ale komunikovat s řadiči domény, které jsou na místě. 

Další informace o optimalizaci sítě najdete v tématu [aspekty topologie sítě, při použití aplikace Proxy Azure Active Directory](application-proxy-network-topology-considerations.md).

## <a name="domain-joining"></a>Připojení k doméně

Konektory lze spustit na počítači, který není připojený k doméně. Pokud chcete jeden přihlašování (SSO) tooapplications, použít integrované ověřování systému Windows (IWA), ale musíte na počítači připojeném k doméně. V takovém případě hello konektor počítače musí být tooa připojené k doméně, která může provádět [Kerberos](https://web.mit.edu/kerberos) omezené delegování jménem uživatelů hello pro hello publikovaným aplikacím.

Konektory lze také připojené k toodomains nebo doménových strukturách, které obsahují částečnou důvěryhodností nebo řadiče domény jen pro tooread.

## <a name="connector-deployments-on-hardened-environments"></a>Nasazení konektoru na posílené prostředí

Obvykle nasazení konektoru je jednoduchý a nepotřebuje žádnou zvláštní konfiguraci. Existují však některé jedinečné podmínky, které musí vzít v úvahu:

* Organizace, které omezit odchozí přenosy hello musí [otevřete požadované porty](active-directory-application-proxy-enable.md#open-your-ports).
* Kompatibilní se standardem FIPS počítače může být požadované toochange jejich konfigurace tooallow hello toogenerate procesy konektoru a uložte certifikát.
* Organizace, které zamknout jejich prostředí na základě procesů hello hello tento problém sítě požadavky mít toomake se, že jsou obě služby konektoru všechny povolené tooaccess požadované porty a adresy IP.
* V některých případech může odchozí dopředného proxy přerušení hello obousměrný certifikát ověřování a způsobit toofail komunikace hello.

## <a name="connector-authentication"></a>Konektor ověřování

tooprovide zabezpečení služby konektory mít tooauthenticate směrem k hello služby a služby hello má tooauthenticate směrem k hello konektor. Toto ověřování se provádí pomocí certifikátů klienta a serveru při hello konektory iniciovat připojení hello. Tento způsob hello správce uživatelské jméno a heslo nejsou uložené na počítači konektor hello.

Hello certifikátů používaných jsou konkrétní toohello Proxy aplikace služby. Budou vytvořeny při počáteční registraci hello a jsou automaticky obnovit hello konektory každých několik měsíců. 

Pokud není připojený konektor služby toohello několik měsíců, vystavení certifikátu může být zastaralý. V takovém případě odinstalujte a znovu nainstalovat registrace tootrigger konektoru hello. Můžete spustit následující příkazy prostředí PowerShell hello:

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

## <a name="under-hello-hood"></a>Pod pokličkou hello

Konektory jsou založené na Windows Server služby Proxy webových aplikací, takže mají většinu hello stejné nástroje pro správu, včetně protokoly událostí systému Windows

 ![Správa protokolů událostí s hello Prohlížeč událostí](./media/application-proxy-understand-connectors/event-view-window.png)

a čítačů výkonu systému Windows. 

 ![Přidat čítače toohello konektor s hello sledování výkonu](./media/application-proxy-understand-connectors/performance-monitor.png)

konektory Hello jste správce a relace protokoly. protokoly správce Hello obsahovat klíče události a jejich chyby. Protokoly relace Hello obsahovat všechny hello transakce a jejich zpracování podrobnosti. 

protokoly hello toosee, přejděte toohello Prohlížeč událostí, otevřete hello **zobrazení** nabídky a povolit **ukazují analytické a ladicí protokoly**. Pak je povolte toostart shromažďování událostí. Tyto protokoly se nezobrazí v Proxy webových aplikací ve Windows serveru 2012 R2 jako hello konektory jsou založené na novější verzi.

Můžete zkontrolovat stav hello služby hello v okně služby hello. konektor Hello zahrnuje dvě služby systému Windows: skutečný konektor hello a aktualizační hello. Oba dva musí spustit celou dobu hello.

 ![Místní služby AzureAD](./media/application-proxy-understand-connectors/aad-connector-services.png)

## <a name="next-steps"></a>Další kroky


* [Publikování aplikací na samostatných sítí a umístění pomocí konektoru skupin](active-directory-application-proxy-connectors-azure-portal.md)
* [Práce s existující místní proxy servery](application-proxy-working-with-proxy-servers.md)
* [Řešení potíží s Proxy aplikace a konektor chyby](active-directory-application-proxy-troubleshoot.md)
* [Jak toosilently instalovat hello konektoru Proxy aplikace služby Azure AD](active-directory-application-proxy-silent-installation.md)

