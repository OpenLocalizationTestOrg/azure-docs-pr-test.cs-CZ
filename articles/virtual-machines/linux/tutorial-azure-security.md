---
title: "aaaAzure Security Center a Linux virtuálních počítačů v Azure | Microsoft Docs"
description: "Další informace o zabezpečení pro virtuální počítač Azure Linux s Azure Security Center."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/07/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7aa0de35fb311457e769f152c8575ec43e41c39c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a>Sledování zabezpečení virtuálního počítače pomocí Azure Security Center

Azure Security Center můžete získat přehled o Azure prostředku postupy zabezpečení. Security Center nabízí integrované zabezpečení monitorování. Může zjistit hrozeb, které jinak může nevšimli. V tomto kurzu se dozvíte o Azure Security Center a postup:
 
> [!div class="checklist"]
> * Nastavení shromažďování dat
> * Nastavte zásady zabezpečení
> * Zobrazení a opravte problémy s konfigurací stavu
> * Zkontrolujte zjištěnými hrozbami  

## <a name="security-center-overview"></a>Security Center – přehled

Security Center identifikuje potenciální potíže s konfigurací virtuálního počítače (VM) a cílem bezpečnostní hrozby. To může zahrnovat virtuální počítače, které chybí skupin zabezpečení sítě, nezašifrované disků a útoku hrubou silou protokol RDP (Remote Desktop). informace Hello je zobrazena na řídicím panelu Security Center hello ve snadno čitelném grafy.

tooaccess hello řídicího panelu Security Center, v hello portál Azure, v nabídce hello, vyberte **Security Center**. Na řídicím panelu hello můžete zobrazit stav zabezpečení hello prostředí Azure, najít počet aktuální doporučení a zobrazit aktuální stav výstrahy threat hello. Každý toosee vysoké úrovně graf můžete rozbalit podrobněji.

![Řídicí panel Security Center](./media/tutorial-azure-security/asc-dash.png)

Security Center překročí data zjišťování tooprovide doporučení pro problémy, které zjistí. Například pokud virtuální počítač byl nasazen bez skupiny zabezpečení služby připojené síti, Security Center zobrazuje doporučení, s nápravy kroky, které můžete provést. Získáte automatizovanou nápravu, aniž byste museli opustit hello kontextu služby Security Center.  

![Doporučení](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a>Nastavení shromažďování dat

Než můžete získat viditelnost do zabezpečení konfigurací virtuálních počítačů, je třeba tooset procesu shromažďování dat Security Center. To zahrnuje zapnutí shromažďování dat a vytvoření data toohold shromážděných účtu úložiště Azure. 

1. Na řídicím panelu Security Center hello, klikněte na tlačítko **zásady zabezpečení**a potom vyberte své předplatné. 
2. Pro **shromažďování dat**, vyberte **na**.
3. Vyberte účet úložiště toocreate **zvolte účet úložiště**. Pak vyberte **OK**.
4. Na hello **zásady zabezpečení** vyberte **Uložit**. 

agenta pro sběr dat Hello Security Center je nainstalován na všech virtuálních počítačů a shromažďování dat začíná. 

## <a name="set-up-a-security-policy"></a>Nastavte zásady zabezpečení

Zásady zabezpečení jsou použité toodefine hello položky, pro které Security Center shromažďuje data a díky doporučení. Můžete použít jiné bezpečnostní zásady toodifferent sady prostředků Azure. I když ve výchozím nastavení jsou prostředky Azure porovnán s všechny položky zásad, můžete vypnout jednotlivé zásady položky pro všechny prostředky Azure nebo pro skupinu prostředků. Podrobné informace o zásadách zabezpečení Security Center najdete v tématu [nastavení zásad zabezpečení v Azure Security Center](../../security-center/security-center-policies.md). 

tooset si zásady zabezpečení pro všechny prostředky Azure:

1. Na řídicím panelu Security Center hello, vyberte **zásady zabezpečení**a potom vyberte své předplatné.
2. Vyberte **zásada Zabránění**.
3. Zapněte nebo vypněte zásady položky, které chcete tooapply tooall prostředků Azure.
4. Jakmile budete hotovi, vyberte nastavení, vyberte **OK**.
5. Na hello **zásady zabezpečení** vyberte **Uložit**. 

tooset si zásadu pro určité skupiny zdrojů:

1. Na řídicím panelu Security Center hello, vyberte **zásady zabezpečení**a pak vyberte skupinu prostředků.
2. Vyberte **zásada Zabránění**.
3. Zapnout nebo vypnout zásady položky, že chcete skupinu prostředků toohello tooapply.
4. V části **DĚDIČNOSTI**, vyberte **jedinečný**.
5. Jakmile budete hotovi, vyberte nastavení, vyberte **OK**.
6. Na hello **zásady zabezpečení** vyberte **Uložit**.  

Také můžete vypnout shromažďování dat pro určité skupiny zdrojů na této stránce.

V následujícím příkladu hello, byl vytvořen jedinečné zásady pro skupinu prostředků s názvem *myResoureGroup*. V této zásadě jsou vypnuté disku šifrování a webové aplikace brány firewall doporučení.

![Jedinečné zásady](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>Zobrazit stav konfigurace virtuálního počítače

Po zapnutá shromažďování dat a nastavit zásadu zabezpečení, Security Center začne tooprovide výstrahy a doporučení. Při nasazování virtuálních počítačů, je nainstalována agenta pro sběr dat hello. Security Center je pak naplněný daty pro hello nové virtuální počítače. Podrobné informace o stavu konfigurace virtuálních počítačů najdete v tématu [chránit virtuální počítače ve službě Security Center](../../security-center/security-center-virtual-machine-recommendations.md). 

Jak se data shromažďují, je agregován hello stav prostředku pro každý virtuální počítač a souvisejících prostředků Azure. Hello informace se zobrazí v grafu aplikace snadno čitelné. 

stav prostředku tooview:

1.  Na řídicím panelu Security Center hello v části **stav zabezpečení prostředků**, vyberte **výpočetní**. 
2.  Na hello **výpočetní** vyberte **virtuální počítače**. Toto zobrazení obsahuje souhrn stavu hello konfigurace pro všechny virtuální počítače.

![Výpočetní stavu](./media/tutorial-azure-security/compute-health.png)

toosee všechna doporučení pro virtuální počítač, vyberte hello virtuálních počítačů. Doporučení a nápravu, jsou popsané v podrobněji hello další části tohoto kurzu.

## <a name="remediate-configuration-issues"></a>Opravit problémy s konfigurací

Po zahájení toopopulate s konfigurační data Security Center doporučení jsou provedená na základě hello zásady zabezpečení, kterou vytvoříte. Pokud virtuální počítač vytvořený bez skupinu zabezpečení sítě spojenou se provádí doporučení pro instanci toocreate jeden. 

toosee seznam všech doporučení: 

1. Na řídicím panelu Security Center hello, vyberte **doporučení**.
2. Vyberte konkrétní doporučení. Zobrazí se seznam všech prostředků, pro kterou platí doporučení hello.
3. tooapply doporučení, vyberte konkrétní prostředku. 
4. Postupujte podle pokynů hello nápravy kroky. 

V mnoha případech Security Center nabízí řešitelné postup můžete provést tooaddress doporučení aniž byste museli opustit Security Center. V následujícím příkladu hello Security Center zjišťuje skupinu zabezpečení sítě, který má neomezený příchozího pravidla. Na stránce hello doporučení, můžete vybrat hello **upravit příchozí pravidla** tlačítko. Zobrazí se uživatelské rozhraní, které je potřebné toomodify hello pravidlo Hello. 

![Doporučení](./media/tutorial-azure-security/remediation.png)

Podle doporučení opraví, jsou označeny jako vyřešené. 

## <a name="view-detected-threats"></a>Zobrazit zjištěnými hrozbami

Kromě toho doporučené konfigurace tooresource, Security Center zobrazuje výstrah o zjištěných hrozbách. Funkce výstrahy zabezpečení Hello agreguje data shromážděná z jednotlivých virtuálních počítačů, Azure síťové protokoly a připojených partnerských řešení toodetect bezpečnostní hrozby proti prostředků Azure. Podrobné informace o možnostech detekce hrozeb Security Center najdete v tématu [funkce zjišťování služby Azure Security Center](../../security-center/security-center-detection-capabilities.md).

Funkce výstrahy zabezpečení Hello vyžaduje hello Security Center cenová úroveň toobe vyšší z *volné* příliš*standardní*. 30denní **bezplatnou zkušební verzi** je k dispozici, když přesouváte toothis vyšší cenová úroveň. 

cenová úroveň toochange hello:  

1. Na řídicím panelu Security Center hello, klikněte na tlačítko **zásady zabezpečení**a potom vyberte své předplatné.
2. Vyberte **cenová úroveň**.
3. Vyberte hello novou vrstvu a pak vyberte **vyberte**.
4. Na hello **zásady zabezpečení** vyberte **Uložit**. 

Po změně hello cenovou úroveň, grafu výstrahy zabezpečení hello začíná toopopulate podle zjištění ohrožení zabezpečení.

![Výstrahy zabezpečení](./media/tutorial-azure-security/security-alerts.png)

Vyberte výstrahy tooview informace. Můžete například zobrazíte popis hello hrozby, čas detekce hello, všechny pokusy hrozeb a hello doporučené nápravy. V následující ukázka hello bylo zjištěno útoku hrubou silou RDP s 294 neúspěšných pokusů protokolu RDP. Doporučené řešení je k dispozici.

![Útok protokolu RDP](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu nastavení Azure Security Center a poté zkontrolovat virtuální počítače ve službě Security Center. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Nastavení shromažďování dat
> * Nastavte zásady zabezpečení
> * Zobrazení a opravte problémy s konfigurací stavu
> * Zkontrolujte zjištěnými hrozbami

Posunutí toohello další kurz toolearn informace o vytváření položek konfigurace nebo CD kanál s volaných, Githubu a Docker.

> [!div class="nextstepaction"]
> [Vytvoření položek konfigurace nebo CD infrastruktury s volaných Githubu a Docker](tutorial-jenkins-github-docker-cicd.md)

