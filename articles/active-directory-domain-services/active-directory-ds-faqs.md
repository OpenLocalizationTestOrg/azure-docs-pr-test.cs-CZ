---
title: "aaaFAQs – Azure Active Directory Domain Services | Microsoft Docs"
description: "Časté otázky k Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 48731820-9e8c-4ec2-95e8-83dba1e58775
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: maheshu
ms.openlocfilehash: 42a6d659f6408bf694cb2aa6ec24bff7a76b0565
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory Domain Services: Časté otázky (FAQ)
Tato stránka odpovědi na nejčastější dotazy týkající hello Azure Active Directory Domain Services. Kontrolovat zpět aktualizací.

### <a name="troubleshooting-guide"></a>Průvodce odstraňováním potíží
Odkazovat tooour [Průvodce odstraňováním potíží](active-directory-ds-troubleshooting.md) pro řešení problémů toocommon zjistil při konfiguraci nebo jejich správě Azure AD Domain Services.

### <a name="configuration"></a>Konfigurace
#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Můžete vytvořit více domén pro jeden adresář Azure AD?
Ne. Můžete vytvořit pouze na jednu doménu, které jsou obsluhovány pomocí Azure AD Domain Services pro jeden adresář Azure AD.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Můžete povolit Azure AD Domain Services ve virtuální síti Azure Resource Manager?
Ne. Služba Azure AD Domain Services lze povolit pouze v klasické virtuální síti Azure. Můžete se připojit hello klasickou virtuální síť tooa Resource Manager virtuální sítě pomocí virtuální sítě partnerského vztahu toouse vaší spravované domény ve virtuální síti Resource Manager.

#### <a name="can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-use-adfs-tooauthenticate-users-for-access-toooffice-365-can-i-enable-azure-ad-domain-services-for-this-directory"></a>Můžete povolit funkci Azure AD Domain Services ve federované Azure AD adresáře? Uživatelé tooauthenticate služby AD FS je možné použít pro přístup k tooOffice 365. Můžete povolit pro tento adresář Azure AD Domain Services?
Ne. Služba Azure AD Domain Services, potřebuje přístup k toohello hodnoty hash hesla uživatelských účtů, tooauthenticate uživatele pomocí protokolu NTLM nebo Kerberos. Federované adresáře hodnot hash hesel se neukládají v hello adresář Azure AD. Azure AD Domain Services se proto nefunguje s takové adresáře Azure AD.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Můžete vytvořit Azure AD Domain Services k dispozici v několika virtuálními sítěmi v rámci Moje předplatné?
samotnou službu Hello přímo nepodporuje tento scénář. Najednou je k dispozici v pouze jednu virtuální síť Azure AD Domain Services. Můžete ale nakonfigurovat připojení mezi více virtuálních sítí tooexpose Azure AD Domain Services tooother virtuálních sítí. Tento článek popisuje, jak můžete [propojit virtuální sítě v Azure](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Můžete povolit Azure AD Domain Services pomocí prostředí PowerShell?
Prostředí PowerShell nebo automatizované nasazení služby Azure AD Domain Services není momentálně k dispozici.

#### <a name="is-azure-ad-domain-services-available-in-hello-new-azure-portal"></a>Azure AD Domain Services je k dispozici v hello nový portál Azure?
Ne. Azure AD Domain Services, lze nastavit pouze v hello [portál Azure classic](https://manage.windowsazure.com). Očekáváme, že podpora tooextend hello [portál Azure](https://portal.azure.com) v budoucnu hello.

#### <a name="can-i-add-domain-controllers-tooan-azure-ad-domain-services-managed-domain"></a>Můžete přidat doménu řadiče tooan Azure AD Domain Services spravované domény?
Ne. Hello domény, které poskytuje Azure AD Domain Services je spravovaná doména. Není nutné tooprovision, konfigurovat nebo jinak spravovat řadiče domény pro tuto doménu - tyto aktivity jsou poskytovány jako služba Microsoft management. Proto nelze přidat další řadiče domény (pro čtení a zápis nebo jen pro čtení) pro hello spravované domény.

### <a name="administration-and-operations"></a>Operace a Správa
#### <a name="can-i-connect-toohello-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Můžete připojit toohello řadič domény pro moje spravované doméně pomocí vzdálené plochy?
Ne. Nemáte oprávnění tooconnect toodomain řadiče pro spravované doméně hello přes vzdálenou plochu. Členové skupiny hello 'Administrators AAD řadič domény, mohou spravovat hello spravované doméně pomocí nástroje pro správu AD například hello Centra správy Active Directory (ADAC) nebo prostředí AD PowerShell. Tyto nástroje se instalují pomocí hello 'nástrojů pro vzdálenou správu, funkce v systému Windows server připojený k spravované doméně toohello.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-toodomain-join-machines-toothis-domain"></a>Povolení I Azure AD Domain Services. Jaké uživatelský účet použijte toodomain spojení počítače toothis domény?
Členové skupiny pro správu hello 'správci AAD řadič domény, můžete počítače připojení k doméně. Kromě toho členy této skupiny mají přístup ke vzdálené ploše toomachines, které byly toohello připojené k doméně.

#### <a name="do-i-have-domain-administrator-privileges-for-hello-managed-domain-provided-by-azure-ad-domain-services"></a>Je nutné oprávnění správce domény pro spravované doméně hello poskytované Azure AD Domain Services?
Ne. Nejsou udělena administrativní oprávnění na hello spravované domény. Oprávnění správce domény a správce podnikové sítě nejsou k dispozici pro toouse v rámci domény hello. Správce stávající domény nebo skupiny správce rozlehlé sítě v rámci adresáře služby Azure AD také nejsou udělena oprávnění správce domény nebo enterprise v doméně hello.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains"></a>Můžete upravit členství ve skupinách pomocí protokolu LDAP nebo jiné nástroje pro správu služby AD na spravované domény?
Ne. V doménách, které jsou obsluhovány pomocí Azure AD Domain Services nelze upravit členství ve skupinách. Hello totéž platí i pro atributy uživatele. Ale může změnit členství ve skupině nebo uživatelské atributy buď ve službě Azure AD, nebo v místní doméně. Tyto změny jsou automaticky synchronizované tooAzure AD Domain Services.

#### <a name="how-long-does-it-take-for-changes-i-make-toomy-azure-ad-directory-toobe-visible-in-my-managed-domain"></a>Jak dlouho bude trvat, změny I zviditelnit toobe directory toomy Azure AD v mé spravované domény?
Změny provedené v adresáři služby Azure AD pomocí hello Azure AD uživatelského rozhraní nebo Powershellu jsou synchronizované tooyour spravované domény. Tento proces synchronizace běží hello pozadí. Po dokončení hello jednorázové počáteční synchronizace adresáře se obvykle trvá přibližně 20 minut, než změny provedené v Azure AD toobe projeví ve vaší spravované domény.

#### <a name="can-i-extend-hello-schema-of-hello-managed-domain-provided-by-azure-ad-domain-services"></a>Můžete rozšířit schéma hello hello spravované doméně poskytované Azure AD Domain Services?
Ne. schéma Hello je možné spravovat Microsoft hello spravované domény. Rozšíření schématu nejsou podporovány službou Azure AD Domain Services.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Můžete změnit nebo přidat záznamy DNS v mé spravované domény?
Ano. Členové skupiny hello 'AAD řadič domény Administrators, jsou udělena oprávnění Správce DNS toomodify DNS zaznamenává ve spravované doméně hello. Uživatelé můžou používat hello konzolu Správce DNS na počítač spuštěný Windows Server připojený k toohello spravované doméně, toomanage DNS. toouse hello konzolu Správce DNS, nainstalujte 'Nástroje serveru DNS', která je součástí hello volitelné funkce, nástroje pro správu vzdáleného serveru, na hello server. Další informace o [nástroje pro správu, monitorování a řešení potíží s DNS](https://technet.microsoft.com/library/cc753579.aspx) je k dispozici na webu TechNet.

### <a name="billing-and-availability"></a>Fakturace a dostupnost
#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Je, že služba Azure AD Domain Services placené služby?
Ano. Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-hello-service"></a>Je k dispozici bezplatná zkušební verze služby hello?
Tato služba je součástí bezplatné zkušební verze hello pro Azure. Můžete si zaregistrovat [bezplatnou zkušební verzi jeden měsíc Azure](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-toouse-azure-ad-domain-services"></a>Můžete získat Azure AD Domain Services v rámci Enterprise Mobility Suite (EMS)? Je nutné Azure AD Premium toouse Azure AD Domain Services?
Ne. Služba Azure AD Domain Services je průběžnými platbami služby Azure a není součástí EMS. Azure AD Domain Services lze ve všech edicích služby Azure AD (volné, základní a, Premium). Fakturuje se na hodinu, v závislosti na využití.

#### <a name="what-azure-regions-is-hello-service-available-in"></a>Jaké oblasti je k dispozici v hello služby?
Odkazovat toohello [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/) hello stránky toosee seznam oblastí Azure, kde je k dispozici služba Azure AD Domain Services.
