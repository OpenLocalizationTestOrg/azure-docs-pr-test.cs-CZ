---
title: "aaaDeploy straně Internetu Vyrovnávání zatížení s IPv6 - šablony Azure | Microsoft Docs"
description: "Jak toodeploy IPv6 podporu pro vyrovnávání zatížení Azure a virtuálních počítačů s vyrovnáváním zatížení."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
keywords: "protokol IPv6, nástroje pro vyrovnávání zatížení azure, duálním zásobníkem, veřejnou IP adresu, nativní protokol ipv6, mobilní, iot"
ms.assetid: 2998e943-13fc-4ea9-a68c-875e53a08db3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2016
ms.author: kumud
ms.openlocfilehash: 68b9ba874a50161243577b64c4a6d9c76b39156c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Nasazení řešení ve Vyrovnávání zatížení internetového s IPv6 pomocí šablony

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Šablona](load-balancer-ipv6-internet-template.md)

Azure Load Balancer je nástroj pro vyrovnávání zatížení úrovně 4 (TCP, UDP). Nástroj pro vyrovnávání zatížení Hello poskytuje vysokou dostupnost distribucí příchozí komunikaci mezi instance pořádku služby ve cloudových službách nebo virtuálních počítačů v sadě nástroje pro vyrovnávání zatížení. Azure Load Balancer můžete také tyto služby prezentovat na více portech, více IP adresách nebo obojím.

## <a name="example-deployment-scenario"></a>Příklad scénáře nasazení

Hello následující diagram znázorňuje řešení vyrovnávání zatížení hello nasazuje pomocí šablony příklad hello popsané v tomto článku.

![Scénář nástroje pro vyrovnávání zatížení](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

V tomto scénáři vytvoříte následující prostředky Azure hello:

* rozhraní virtuální sítě pro každý virtuální počítač s oba protokoly IPv4 a IPv6 adresy přiřazené
* Vyrovnávání zatížení straně Internetu s IPv4 a IPv6 veřejnou IP adresu
* dva načíst vyrovnávání pravidla toomap hello veřejné VIP toohello privátní koncové body
* Skupiny dostupnosti, která obsahuje hello dva virtuální počítače
* dva virtuální počítače (VM)

## <a name="deploying-hello-template-using-hello-azure-portal"></a>Nasazení šablony hello pomocí hello portálu Azure

Tento článek odkazuje na šablonu, která je publikovaná v hello [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) galerie. Můžete stáhnout hello šablonu z Galerie hello nebo spusťte hello nasazení v Azure přímo z Galerie hello. Tento článek předpokládá, že jste si stáhli hello šablony tooyour místního počítače.

1. Otevřete hello portál Azure a přihlaste se pomocí účtu, který má oprávnění toocreate virtuálních počítačů a síťových prostředků v rámci předplatného Azure. Navíc pokud používáte stávající prostředky, hello účet potřebuje oprávnění toocreate skupinu prostředků a účet úložiště.
2. Klikněte na tlačítko "+ nový" z nabídky hello pak typu "Šablona" hello vyhledávacího pole. Vyberte z výsledků hledání hello "Nasazení šablony".

    ![LB-ipv6 – portál – krok 2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. V hello všechno okně klikněte na tlačítko "Šablonu nasazení."

    ![LB ipv6 portál krok3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Klikněte na tlačítko "Vytvořit".

    ![LB ipv6 portál step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Klikněte na "Upravit šablonu." Odstraňte existující obsah hello, kopírování a vkládání v hello celý obsah souboru šablony hello (tooinclude hello spuštění a ukončení {}), pak klikněte na tlačítko "Uložit."

    > [!NOTE]
    > Pokud používáte Microsoft Internet Explorer, když vložíte můžete zobrazit dialogové okno s dotazem, schránky systému Windows toohello tooallow přístup. Klikněte na možnost "Povolit přístup."

    ![LB ipv6 portál step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Klikněte na "Upravit parametry." V okně hello parametry zadejte hodnoty hello podle pokynů hello v hello oddíle Parametry šablony a pak klikněte na "Uložit" tooclose hello parametry okno. V okně Vlastní nasazení hello vyberte předplatné, existující skupinu prostředků nebo vytvořit. Pokud vytváříte skupinu prostředků, pak vyberte umístění pro skupinu prostředků hello. Klikněte na tlačítko **právní podmínky**, pak klikněte na tlačítko **nákupu** pro hello právní podmínky. Azure začne nasazení prostředků hello. Trvá několik minut toodeploy všechny prostředky hello.

    ![LB ipv6 portál step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Další informace o těchto parametrů najdete v tématu hello [šablony parametry a proměnné](#template-parameters-and-variables) později v tomto článku.

7. prostředky hello toosee vytvořené šablony hello, klikněte na tlačítko Procházet, přejděte dolů na seznamu hello najdete v části "Prostředku skupiny" a pak klikněte na něj.

    ![LB ipv6 portál step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. V okně skupiny prostředků hello klikněte na tlačítko hello název skupiny prostředků hello, který jste zadali v kroku 6. Zobrazí seznam všech hello prostředků, které byly nasazeny. Pokud se všechny správně, je by mělo být uvedeno "Succeeded" v části "Posledního nasazení." Pokud ne, zkontrolujte, zda text hello účet, který používáte oprávnění toocreate hello potřebné prostředky.

    ![LB ipv6 portál step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [!NOTE]
    > Pokud hned po dokončení kroku 6 vaší skupiny prostředků, zobrazí "Posledního nasazení" hello stav "Nasazení" hello prostředky se se nasadí.

9. Klikněte na tlačítko "myIPv6PublicIP" hello seznamu prostředků. Uvidíte, že má adresu IPv6 v rámci IP adresu, že jeho název DNS je hello hodnotu, kterou jste zadali pro parametr dnsNameforIPv6LbIP hello v kroku 6. Tento prostředek je hello veřejné IPv6 adresy a názvu hostitele, který je přístupný tooInternet – klienti.

    ![LB ipv6 portál step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Ověření připojení

Když hello šablony je nasazena úspěšně, můžete ověřit připojení provedením hello následující úlohy:

1. Přihlaste se toohello portál Azure a připojte tooeach hello virtuální počítače vytvořené nasazení šablony hello. Pokud jste nasadili virtuální počítač Windows serveru, spusťte příkaz ipconfig/všechny z příkazového řádku. Uvidíte, že virtuální počítače hello mají adresy IPv4 a IPv6. Pokud nasadíte virtuální počítače s Linuxem, je třeba tooconfigure hello operační systém Linux tooreceive dynamické adresy IPv6 pomocí hello pokynů pro vaše distribuci systému Linux.
2. Z klienta připojeného k Internetu IPv6 Inicializujte připojení toohello veřejná IPv6 adresa služby Vyrovnávání zatížení hello. tooconfirm, který hello nástroj pro vyrovnávání zatížení je vyrovnávání mezi hello dva virtuální počítače, můžete nainstalovat webový server jako internetové informační služby (IIS) na každém hello virtuálních počítačů. Hello výchozí webové stránky na každém serveru můžou obsahovat text hello "Server0" nebo "Server1" toouniquely ho identifikovat. Poté otevřete internetový prohlížeč na klientech připojené k Internetu IPv6 a procházet toohello název hostitele, zadaná pro parametr dnsNameforIPv6LbIP hello hello zatížení vyrovnávání tooconfirm začátku do konce IPv6 připojení tooeach virtuálních počítačů. Pokud se zobrazí pouze hello webové stránky pouze jeden server, musíte tooclear vaší mezipaměti prohlížeče. Otevřete více privátní procházení relací. Měli byste vidět odpovědi z jednotlivých serverů.
3. Z klienta připojeného k Internetu IPv4 Inicializujte připojení toohello veřejnou IPv4 adresu služby Vyrovnávání zatížení hello. tooconfirm, který hello nástroj pro vyrovnávání zatížení je Vyrovnávání zatížení hello dva virtuální počítače, může test pomocí služby IIS, jak je podrobně uvedeno v kroku 2.
4. Z jednotlivých virtuálních počítačů inicializujte tooan odchozí připojení IPv6 nebo zařízení připojená k IPv4 Internetové aplikace. V obou případech je hello zdrojové IP adresy pohledu hello cílové zařízení hello veřejnou IPv4 nebo IPv6 adresu služby Vyrovnávání zatížení hello.

> [!NOTE]
> ICMP pro oba protokoly IPv4 a IPv6 je blokována v hello síť Azure. V důsledku toho ICMP nástroje jako vždy ping nezdaří. tootest připojení, použijte alternativní TCP například TCPing nebo hello rutině Test NetConnection prostředí PowerShell. Všimněte si, že hello IP adresy uvedené v diagramu hello jsou příklady hodnot, které se můžou objevit. Vzhledem k tomu, že hello IPv6 adresy přiřazené dynamicky, hello adresy, které se zobrazí, se budou lišit a může se liší podle oblasti. Navíc je běžné hello veřejnou adresu IPv6 na toostart nástroje pro vyrovnávání zatížení hello s jinou než hello privátní adresy IPv6 ve fondu back-end hello.

## <a name="template-parameters-and-variables"></a>Parametry šablony a proměnné

Šablonu Azure Resource Manager obsahuje více proměnných a parametry, můžete přizpůsobit potřebám tooyour. Proměnné se používají pro pevné hodnoty, které nechcete toochange uživatele. Parametry se používají pro hodnoty, který má tooprovide uživatele při nasazování šablony hello. Příklad šablony Hello je nakonfigurován pro hello scénář popsaný v tomto článku. Můžete upravit tento tooneeds vašeho prostředí.

Příklad Hello šablony použité v tomto článku zahrnuje následující hello parametry a proměnné:

| Parametr / proměnné | Poznámky |
| --- | --- |
| adminUsername |Zadejte název hello účtu správce hello používá toosign v toohello virtuálních počítačů s. |
| adminPassword |Zadejte hello heslo pro účet správce hello používá toosign v toohello virtuálních počítačů s. |
| dnsNameforIPv4LbIP |Zadejte název hostitele DNS hello chcete tooassign jako hello veřejný název služby Vyrovnávání zatížení hello. Tento název přeloží toohello služby Vyrovnávání zatížení veřejnou adresu IPv4. Název Hello musí být psaný malými písmeny a odpovídají hello regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP |Zadejte název hostitele DNS hello chcete tooassign jako hello veřejný název služby Vyrovnávání zatížení hello. Tento název přeloží toohello služby Vyrovnávání zatížení veřejnou adresu IPv6. Název Hello musí být psaný malými písmeny a odpovídají hello regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. To může být hello stejný název jako hello adresu IPv4. Když klient odešle dotaz DNS pro tento název Azure bude vracet hello A a AAAA záznamy se sdílí hello název. |
| vmNamePrefix |Zadejte předponu názvu hello virtuálních počítačů. Šablona Hello připojí číslo (0, 1, atd.) název toohello při hello virtuální počítače jsou vytvořeny. |
| nicNamePrefix |Zadejte předponu názvu rozhraní sítě hello. Šablona Hello připojí číslo (0, 1, atd.) název toohello při vytvoření hello síťových rozhraní. |
| storageAccountName |Zadejte název hello stávající účet úložiště nebo zadejte název nové jeden toobe vytvořené šablony hello hello. |
| availabilitySetName |Zadejte název hello dostupnosti nastavena toobe použít s hello virtuální počítače |
| addressPrefix |Předpona adresy Hello používá rozsah adres hello toodefine hello virtuální sítě |
| subnetName |pro hello virtuální sítě vytvoří Hello název podsítě hello v |
| subnetPrefix |Předpona adresy Hello používá rozsah adres hello toodefine hello podsítě |
| vnetName |Zadejte název hello hello virtuální sítě používá hello virtuálních počítačů. |
| ipv4PrivateIPAddressType |Metoda přidělení Hello používá pro hello privátní IP adresa (statické nebo dynamické) |
| ipv6PrivateIPAddressType |Metoda přidělení Hello používá pro hello privátní IP adresa (dynamické). Dynamické přidělení podporuje pouze protokol IPv6. |
| numberOfInstances |Hello počet zatížení vyrovnáváním instance nasazené šablonou hello |
| ipv4PublicIPAddressName |Zadejte název DNS hello chcete toouse toocommunicate s hello veřejnou IPv4 adresu služby Vyrovnávání zatížení hello. |
| ipv4PublicIPAddressType |Metoda přidělení Hello používá pro hello veřejnou IP adresu (statické nebo dynamické) |
| Ipv6PublicIPAddressName |Zadejte název DNS hello chcete toouse toocommunicate s veřejnou adresou IPv6 hello nástroje pro vyrovnávání zatížení hello. |
| ipv6PublicIPAddressType |Metoda přidělení Hello použitá pro hello veřejnou IP adresu (dynamické). Dynamické přidělení podporuje pouze protokol IPv6. |
| lbName |Zadejte název hello nástroje pro vyrovnávání zatížení hello. Tento název se zobrazí v aplikaci portál hello nebo použité při odkazu tooit pomocí rozhraní CLI nebo Powershellu příkazu. |

Hello zbývající variables v šabloně hello obsahovat odvozené hodnoty, které jsou přiřazeny, když vytváří hello prostředky Azure. Neměňte tyto proměnné.
