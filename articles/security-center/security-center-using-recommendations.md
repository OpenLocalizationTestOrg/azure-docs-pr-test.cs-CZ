---
title: "aaaUse zabezpečení tooenhance doporučení Azure Security Center | Microsoft Docs"
description: " Zjistěte, jak toouse zásady a doporučení zabezpečení v Azure Security Center toohelp zmírnit útok na zabezpečení. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: terrylan
ms.openlocfilehash: bd314350a5abfceea3e171f2e1b55afe4549c1b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-security-center-recommendations-tooenhance-security"></a>Použití Azure Security Center doporučení tooenhance zabezpečení
Můžete snížit pravděpodobnost hello událostí významné zabezpečení tak, že konfigurace zásad zabezpečení a pak implementace hello doporučení pomocí služby Azure Security Center. Tento článek ukazuje, jak toouse zásady a doporučení zabezpečení v Security Center toohelp zmírnit útok na zabezpečení.

> [!NOTE]
> Tento článek vychází hello role a koncepty představené v hello Security Center [Průvodce plánováním a operace](security-center-planning-and-operations-guide.md). Před pokračováním je Průvodce plánování vhodné tooreview hello.
>
>

## <a name="managing-security-recommendations"></a>Správa doporučení zabezpečení
Zásady zabezpečení definuje hello sadu ovládacích prvků, které se doporučují pro prostředky v rámci zadané předplatné nebo prostředek skupiny hello. V Security Center určíte zásady podle tooyour požadavky na zabezpečení společnosti. Další, najdete v části toolearn [nastavovat zásady zabezpečení ve službě Security Center](security-center-policies.md).

Zásady zabezpečení pro skupiny prostředků dědí z úrovně předplatného hello.

![Dědičnost zásad zabezpečení][1]

Pokud potřebujete vlastní zásady v určitých skupinách prostředků, můžete zakázat dědičnost ve skupině prostředků hello. toodisable, nastavte dědičnost tooUnique hello okna zásady zabezpečení a přizpůsobit hello ovládací prvky, které Security Center zobrazuje doporučení pro.

Například pokud máte úlohy, které nevyžadují zásadu hello SQL databáze transparentní dat šifrování (TDE), vypněte hello zásad na úrovni předplatného hello a povolte ji jenom ve skupinách prostředků hello, ve kterých je požadovaná.

> [!NOTE]
> Pokud dojde ke konfliktu mezi zásadou na úrovni předplatného a zásadou na úrovni skupiny prostředků, úrovni zásady skupiny prostředků hello přednost.
>
>

Security Center analyzuje stav zabezpečení hello vašich prostředků Azure. Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří se doporučení podle nastavení v zásadách zabezpečení hello hello – ovládací prvky. Hello doporučení vás provede procesem hello konfigurace ovládacích prvků zabezpečení hello potřeby.

Aktuální zásady doporučení v Security Center se zaměřují na aktualizací systému, konfigurace operačního systému, skupin zabezpečení podsítí a virtuálních počítačů (VM), auditování databáze SQL TDE databáze SQL, sítě a brány firewall webových aplikací. Hello nejaktuálnější pokrytí doporučení služby Security Center, najdete v části [Správa doporučení zabezpečení v Centru zabezpečení](security-center-recommendations.md).

## <a name="scenario"></a>Scénář
Tento scénář popisuje, jak toouse Security Center toohelp omezit hello pravděpodobnost incidentu významné zabezpečení tím, že monitorování doporučení služby Security Center a akce. Hello scénář používá hello fiktivní společnost Contoso, a rolí, které jsou uvedené v hello Security Center [Průvodce plánováním a operace](security-center-planning-and-operations-guide.md#security-roles-and-access-controls). Hello role představují jednotlivce a týmy, které mohou používat Security Center tooperform různé související se zabezpečením úlohy. Hello role jsou:

![Scénář role][2]

Contoso nedávno migrace některé z jejich tooAzure místní prostředky. Contoso chce tooimplement a zachovat ochranu, která snižují útoku zabezpečení tooa ohrožení zabezpečení svých prostředků v cloudu hello.

## <a name="recommended-solution"></a>Doporučené řešení
Řešení je tooprevent toouse Security Center a zjištění chyb zabezpečení. Contoso má Centrum tooSecurity přístupu prostřednictvím svého předplatného Azure. Hello [úroveň Free](security-center-pricing.md) služby Security Center je automaticky povolené na všechny odběry služby Azure a shromažďování dat je povolené na všech virtuálních počítačích v rámci svého předplatného.

Nakonfiguruje David v rámci zabezpečení IT společnosti Contoso **zásady zabezpečení** pomocí Security Center. Security Center analyzuje stav zabezpečení prostředků Azure společnosti Contoso hello. Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří **doporučení** podle nastavení v zásadách zabezpečení hello hello – ovládací prvky.

Jeff, vlastník úloh v cloudu, je zodpovědný za implementaci a udržování ochrany v souladu se zásadami zabezpečení společnosti Contoso. Jeff můžete monitorovat vytvořené Security Center tooapply ochrana doporučení hello. doporučení Hello provede Jeff hello proces konfigurace ovládacích prvků zabezpečení hello potřeby.

V pořadí pro Jeff tooimplement a udržovat ochrany a odstranění chyby zabezpečení, he musí:

- Monitorování doporučení zabezpečení pomocí služby Security Center
- Vyhodnocení doporučení zabezpečení a rozhodnout, pokud mu použít nebo vymazat
- Použít doporučení zabezpečení

Pojďme postupujte podle kroků toosee Jeff jak pomocí Security Center doporučení tooguide mu procesem hello konfigurace ohrožení zabezpečení tooeliminate ovládací prvky.

## <a name="how-tooimplement-this-solution"></a>Jak tooimplement toto řešení
Jeff přihlásí příliš[portál Azure](https://azure.microsoft.com/features/azure-portal/) a otevře se okno hello konzole Security Center. V rámci jeho denní monitorování aktivit mohl zkontroluje toosee, pokud jsou doporučení zabezpečení provedením následujících kroků hello:

1. Jan vybere hello **doporučení** dlaždice tooopen hello **doporučení** okno.
   ![Vyberte hello doporučení dlaždice][3]
2. Jan zkontroluje hello seznam doporučení. Zjistí, že Security Center poskytl hello seznam doporučení v pořadí podle priority z nejvyšší prioritou toolowest s prioritou. Rozhodne tooaddress doporučení vysokou prioritu na seznamu hello. Vybere **nainstalovat službu Endpoint Protection** na hello **doporučení** okno.
3. Hello **nainstalovat službu Endpoint Protection** otevře se okno zobrazení seznamu virtuálních počítačů bez antimalwarových povolena. Jan zkontroluje hello seznam virtuálních počítačů, vybere všechny virtuální počítače a potom vybere **nainstalovat na virtuálních počítačích 3**.
   ![Instalace Endpoint Protection][4]
4. Hello **vyberte Endpoint Protection** poskytnete Jeff otevře se okno s dvěma antimalwarových řešení. Jan vybere hello **Antimalware od Microsoftu** řešení.
5. Další informace o řešení proti malwaru hello se zobrazí. Jan vybere **vytvořit**.
   ![Antimalware od Microsoftu][5]
6. Jan zadá hello požadované nastavení konfigurace na hello **nainstalovat** okno a vybere **OK**.

[Microsoft Antimalware](../security/azure-security-antimalware.md) je teď aktivní na hello vybrané virtuální počítače.

Jeff pokračuje toomove prostřednictvím hello s vysokou prioritou a doporučení se střední prioritou, při rozhodování o provádění. Jeff odkazuje hello [Správa doporučení zabezpečení](security-center-recommendations.md) článek toounderstand hello doporučení a co každé z nich dělá pokud mu použije ji.

Jeff zjišťuje, které [Microsoft Security Response Center (MSRC)](../security/azure-security-response-center.md) provede vyberte zabezpečení monitorování hello síť Azure a infrastruktury a přijímá stížností intelligence a zneužití ohrožení od jiných výrobců. Pokud Jeff poskytuje zabezpečení kontaktní údaje pro předplatné Azure společnosti Contoso, kontakty Microsoft Contoso Pokud hello střediska MSRC zjišťuje data zákazníků společnosti Contoso přístupem k nezákonné nebo neoprávněným strana. Pojďme postupujte Jeff podle mu platí hello **zadejte kontaktní údaje zabezpečení** doporučení (má tyto požadavky se závažností střední v hello seznam doporučení výše).

1. Jan vybere **zadejte kontaktní údaje zabezpečení** na hello **doporučení** okno otevře hello **zadejte kontaktní údaje zabezpečení** okno.
2. Jan vybere hello předplatného Azure tooprovide kontaktní informace na. Druhý **zadejte kontaktní údaje zabezpečení** otevře se okno.
   ![Kontaktní údaje zabezpečení][6]
3. Na hello druhý **zadejte kontaktní údaje zabezpečení** zadá Jeff okně:

  - Hello zabezpečení kontaktní e-mailových adres oddělených čárkami (není číslo toohello limit e-mailových adres, které může zadat)
  - telefonní číslo kontaktu jeden zabezpečení

4. Jan také zapne hello možnost **zaslat mi e-maily o výstrahách** tooreceive e-mailů o výstrahy s vysokou závažností.
5. Jan vybere **OK** tooapply hello zabezpečení obraťte se na informace tooContoso předplatné.

Nakonec Jeff zkontroluje doporučení s nízkou prioritou hello **ohrožení zabezpečení operačního systému opravit** a určuje, že toto doporučení se nedá použít. Chce toodismiss hello doporučení. Jan vybere hello tři tečky, které se zobrazují toohello práva a potom vybere **přeskočení**.
   ![Zavření doporučení][7]

## <a name="conclusion"></a>Závěr
Monitorování doporučení ve službě Security Center vám mohou pomoci eliminovat ohrožení zabezpečení, než dojde k útoku. Můžete-li zabránit bezpečnostního incidentu, implementaci a udržování ochrany zásadám zabezpečení ve službě Security Center.

<!--Image references-->
[1]: ./media/security-center-using-recommendations/security-center-policy-inheritance.png
[2]: ./media/security-center-using-recommendations/scenario-roles.png
[3]: ./media/security-center-using-recommendations/select-recommendations-tile.png
[4]: ./media/security-center-using-recommendations/install-endpoint-protection.png
[5]:./media/security-center-using-recommendations/microsoft-antimalware.png
[6]: ./media/security-center-using-recommendations/provide-security-contact-details.png
[7]: ./media/security-center-using-recommendations/dismiss-recommendation.png
