# <a name="frequently-asked-questions-about-classic-tooazure-resource-manager-migration"></a>Nejčastější dotazy o classic tooAzure při migraci správce prostředků

## <a name="does-this-migration-plan-affect-any-of-my-existing-services-or-applications-that-run-on-azure-virtual-machines"></a>Má tento plán migrace vliv na některé stávající služby nebo aplikace spuštěné na virtuálních počítačích Azure? 

Ne. virtuální počítače Hello (klasické) jsou plně podporované služby v obecné dostupnosti. Můžete pokračovat toouse tyto prostředky tooexpand vaše nároky na Microsoft Azure.

## <a name="what-happens-toomy-vms-if-i-dont-plan-on-migrating-in-hello-near-future"></a>Pokud nemáte plánujete migraci v blízké budoucnosti hello co se stane toomy virtuální počítače? 

Jsme nejsou místo začne hello existující klasické rozhraní API a prostředků model. Migrace toomake chceme snadno, s ohledem hello pokročilé funkce, které jsou k dispozici v modelu nasazení Resource Manager hello. Důrazně doporučujeme, abyste si prošli [některé hello rozvoj](../articles/azure-resource-manager/resource-manager-deployment-model.md) které jsou součástí IaaS v části správce prostředků.

## <a name="what-does-this-migration-plan-mean-for-my-existing-tooling"></a>Co tento plán migrace znamená pro stávající nástroje? 

Aktualizace modelu nasazení Resource Manager toohello nástrojů je jedním z hello nejdůležitější změny, které máte v plánu migrace tooaccount pro.

## <a name="how-long-will-hello-management-plane-downtime-be"></a>Jak dlouho hello správu roviny výpadek bude? 

To závisí na hello počet prostředků, které se migruje. Pro menší nasazení (několik desítek virtuálních počítačů) by měl hello celou migrace trvat méně než jednu hodinu. Pro rozsáhlá nasazení (stovky virtuálních počítačů) hello migrace může trvat několik hodin.

## <a name="can-i-roll-back-after-my-migrating-resources-are-committed-in-resource-manager"></a>Dají se vrátit změny po potvrzení migrovaných prostředků v Resource Manageru? 

Migrace může přerušit, dokud hello prostředky jsou v hello připravený stavu. Vrácení zpět není podporována po hello prostředkům prostřednictvím operace potvrzení hello úspěšně migraci.

## <a name="can-i-roll-back-my-migration-if-hello-commit-operation-fails"></a>Můžete I vrátit zpět, Moje migrace Pokud selže operace potvrzení hello? 

Migrace nelze přerušit, pokud se nezdaří operace potvrzení hello. Všech operací migrace, včetně hello operace potvrzení, jsou idempotent. Proto doporučujeme, abyste po krátkou dobu hello operaci zkuste zopakovat. Pokud stále musí čelit chybu, vytvořit lístek podpory, nebo vytvořit příspěvek ve fóru s hello ClassicIaaSMigration značky na našem [fórum virtuálních počítačů](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).

## <a name="do-i-have-toobuy-another-express-route-circuit-if-i-have-toouse-iaas-under-resource-manager"></a>Je nutné provést toobuy jiného okruhu express route Pokud toouse IaaS v části Resource Manager? 

Ne. Nedávno povolené [přesun okruhů ExpressRoute z modelu nasazení Resource Manager classic toohello hello](../articles/expressroute/expressroute-move.md). Nemáte toobuy nové okruh ExpressRoute Pokud již účet máte.

## <a name="what-if-i-had-configured-role-based-access-control-policies-for-my-classic-iaas-resources"></a>Co když byly pro prostředky IaaS v modelu Classic nakonfigurované zásady řízení přístupu na základě role? 

Během migrace transformuje hello prostředky classic tooResource správce. Proto doporučujeme, abyste naplánovali hello RBAC zásady aktualizace, které vyžadují toohappen po migraci.

## <a name="i-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-tooresource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Svoje klasické virtuální počítače jsem zálohoval do trezoru služby Backup. Provést migraci virtuálních počítačů z klasického režimu tooResource Manager režimu a jejich ochraně v trezoru služeb zotavení? 

Classic body obnovení virtuálního počítače v trezoru záloh není migrovat automaticky tooa trezor služeb zotavení, když přesouváte hello virtuálních počítačů z classic tooResource režimu správce. Postupujte podle těchto kroků tootransfer zálohování virtuálních počítačů:

1. V úložišti záloh hello, přejděte toohello **chráněné položky** a vyberte hello virtuálních počítačů. Klikněte na [Zastavit ochranu](../articles/backup/backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Políčko *Delete associated backup data* (Odstranit přidružená data záloh) ponechte **nezaškrtnuté**.
2. Odstraňte rozšíření zálohování nebo snímek hello z hello virtuálních počítačů.
3. Migrujte hello virtuální počítač z režimu správce tooResource klasickém režimu. Ujistěte se, zda text hello úložiště a informace o síti, odpovídající toohello virtuální počítač je taky migrovat tooResource režimu správce.
4. Vytvoření trezoru služeb zotavení a konfigurace virtuálního počítače pomocí zálohování na hello migrovat **zálohování** akce nad panelu trezoru. Podrobné informace o zálohování virtuální počítač tooa obnovení služby trezoru, najdete v článku hello [chránit virtuální počítače Azure s trezoru služeb zotavení](../articles/backup/backup-azure-vms-first-look-arm.md).

## <a name="can-i-validate-my-subscription-or-resources-toosee-if-theyre-capable-of-migration"></a>Můžete I ověřit Moje předplatné nebo prostředky toosee Pokud podporující migrace? 

Ano. V možnosti podporované platformy migrace hello je hello prvním krokem při přípravě na migraci toovalidate migrace podporují hello prostředky. V případě, že hello ověřit operace selže, zobrazí se zprávy důvodů hello, hello migrace nelze dokončit.

## <a name="what-happens-if-i-run-into-a-quota-error-while-preparing-hello-iaas-resources-for-migration"></a>Co se stane, když spustím kvóty chyba během přípravy prostředky infrastruktury hello migraci? 

Doporučujeme, abyste abort migrace a potom se přihlaste podporu požadavku tooincrease hello kvóty v hello oblasti, kde jsou hello migraci virtuálních počítačů. Po schválení žádosti kvóty hello se můžete spustit provedením kroků migrace hello znovu.

## <a name="how-do-i-report-an-issue"></a>Jak se dá nahlásit problém? 

POST problémy a otázky týkající se migrace tooour [virtuálních počítačů fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows), s hello – klíčové slovo ClassicIaaSMigration. Doporučujeme, abyste všechny vaše dotazy zveřejnili na tomto fóru. Pokud máte kontraktu podpory, jste úvodní toolog lístek podpory.

## <a name="what-if-i-dont-like-hello-names-of-hello-resources-that-hello-platform-chose-during-migration"></a>Co když nelíbí hello názvy hello prostředky, které hello platformy zvolili během migrace? 

Během migrace se zachovají všechny hello prostředky, které explicitně zadáte názvy v modelu nasazení classic hello. V některých případech se vytvoří nové prostředky. Příklad: pro každý virtuální počítač se vytvoří síťové rozhraní. Aktuálně nepodporujeme hello možnost toocontrol hello názvy těchto nových prostředků vytvořili během migrace. Protokolu vaší hlasů pro tuto funkci hello [fóru pro zpětnou vazbu Azure](http://feedback.azure.com).

## <a name="can-i-migrate-expressroute-circuits-used-across-subscriptions-with-authorization-links"></a>Je možné migrovat okruhy ExpressRoute používané napříč předplatnými pomocí autorizačních odkazů? 

Okruhy ExpressRoute používající autorizační odkazy mezi předplatnými není možné automaticky migrovat bez výpadku. Nabízíme doprovodné materiály popisující ruční postup jejich migrace. V tématu [migrovat ExpressRoute okruhy a přidružené virtuální sítě z modelu nasazení Resource Manager classic toohello hello](../articles/expressroute/expressroute-migration-classic-resource-manager.md) kroky a další informace.

## <a name="i-got-a-message-vm-is-reporting-hello-overall-agent-status-as-not-ready-hence-hello-vm-cannot-be-migrated-ensure-that-hello-vm-agent-is-reporting-overall-agent-status-as-ready-or-vm-contains-extension-whose-status-is-not-being-reported-from-hello-vm-hence-this-vm-cannot-be-migrated-"></a>Zobrazí chybové hlášení *"virtuálního počítače je generování sestav hello celkový stav agenta jako není připravené. Proto se nedají migrovat hello virtuálních počítačů. Ujistěte se, že hello agenta virtuálního počítače je generování sestav celkového stavu agenta jako připravené"* nebo *"virtuální počítač obsahuje rozšíření, jejichž stav není nehlásí z hello virtuálních počítačů. Virtuální počítač proto není možné migrovat.“ *

Tato zpráva je Doručená hello virtuální počítač nemá toohello odchozí připojení k Internetu. agent virtuálního počítače Hello používá účet úložiště Azure hello tooreach odchozí připojení pro aktualizaci stavu agenta hello každých pět minut.
