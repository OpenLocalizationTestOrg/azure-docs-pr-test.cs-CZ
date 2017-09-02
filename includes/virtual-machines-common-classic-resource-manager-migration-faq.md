# <a name="frequently-asked-questions-about-classic-to-azure-resource-manager-migration"></a>Nejčastější dotazy ohledně migrace z modelu Classic na Azure Resource Manager

## <a name="does-this-migration-plan-affect-any-of-my-existing-services-or-applications-that-run-on-azure-virtual-machines"></a>Má tento plán migrace vliv na některé stávající služby nebo aplikace spuštěné na virtuálních počítačích Azure? 

Ne. Virtuální počítače (Classic) jsou plně podporované a obecně dostupné služby. I nadále můžete tyto prostředky používat k rozšíření vaší působnosti v systému Microsoft Azure.

## <a name="what-happens-to-my-vms-if-i-dont-plan-on-migrating-in-the-near-future"></a>Co se stane s virtuálními počítači, pokud se je nechystám migrovat v blízké budoucnosti? 

Stávající rozhraní API ani model prostředků Classic nepřestáváme podporovat. Vzhledem k pokročilým funkcím dostupným v modelu nasazení Resource Manager chceme migraci usnadnit. Důrazně doporučujeme, abyste si prošli [některá ze zlepšení](../articles/azure-resource-manager/resource-manager-deployment-model.md), která jsou součástí infrastruktury jako služby (IaaS) v modelu Resource Manager.

## <a name="what-does-this-migration-plan-mean-for-my-existing-tooling"></a>Co tento plán migrace znamená pro stávající nástroje? 

Aktualizace nástrojů na model nasazení Resource Manager je jedna z nejdůležitějších změn, se kterou v plánech migrace musíte počítat.

## <a name="how-long-will-the-management-plane-downtime-be"></a>Jak dlouho bude trvat výpadek roviny správy? 

To závisí na počtu prostředků, které se migrují. U menších nasazení (několik desítek virtuálních počítačů) by celá migrace měla trvat méně než hodinu. U rozsáhlých nasazení (stovky virtuálních počítačů) může migrace trvat několik hodin.

## <a name="can-i-roll-back-after-my-migrating-resources-are-committed-in-resource-manager"></a>Dají se vrátit změny po potvrzení migrovaných prostředků v Resource Manageru? 

Migraci můžete přerušit, dokud jsou prostředky v připraveném stavu. Po úspěšné migraci prostředků prostřednictvím operace potvrzení se vrácení zpět nepodporuje.

## <a name="can-i-roll-back-my-migration-if-the-commit-operation-fails"></a>Je možné vrátit migraci zpět v případě, že selže operace potvrzení? 

Pokud selže operace potvrzení, migraci není možné přerušit. Všechny operace migrace, včetně operace potvrzení, jsou idempotentní. Proto doporučujeme, abyste operaci po chvilce zkusili opakovat. Pokud stále dochází k chybě, vytvořte lístek podpory nebo na našem [fóru k virtuálním počítačům](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows) vytvořte příspěvek se značkou ClassicIaaSMigration.

## <a name="do-i-have-to-buy-another-express-route-circuit-if-i-have-to-use-iaas-under-resource-manager"></a>Je nutné koupit další okruh ExpressRoute, když potřebuji používat infrastrukturu jako službu (IaaS) v modelu Resource Manager? 

Ne. Nedávno jsme povolili [přesun okruhů ExpressRoute z modelu nasazení Classic na Resource Manager](../articles/expressroute/expressroute-move.md). Pokud už máte okruh ExpressRoute, není nutné kupovat nový.

## <a name="what-if-i-had-configured-role-based-access-control-policies-for-my-classic-iaas-resources"></a>Co když byly pro prostředky IaaS v modelu Classic nakonfigurované zásady řízení přístupu na základě role? 

Během migrace se prostředky transformují z modelu Classic na Resource Manager. Proto doporučujeme naplánovat aktualizace zásad řízení přístupu na základě role, které je třeba provést, až po migraci.

## <a name="i-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Svoje klasické virtuální počítače jsem zálohoval do trezoru služby Backup. Můžu migrovat svoje virtuální počítače z klasického režimu do režimu Resource Manageru a chránit je pomocí trezoru služby Recovery Services? 

Při převádění virtuálních počítačů z klasického režimu do režimu Resource Manageru Body neproběhne automatická migrace bodů obnovení klasických virtuálních počítačů do trezoru služby Recovery Services. Při převedení záloh virtuálních počítačů použijte tento postup:

1. V trezoru služby Backup přejděte na kartu **Chráněné položky** a vyberte virtuální počítač. Klikněte na [Zastavit ochranu](../articles/backup/backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Políčko *Delete associated backup data* (Odstranit přidružená data záloh) ponechte **nezaškrtnuté**.
2. Odstraňte z virtuálního počítače zálohu/rozšíření snímků.
3. Proveďte migraci virtuálního počítače z klasického režimu do režimu Resource Manageru. Ověřte, že se do režimu Resource Manager migruje také úložiště a informace o síti odpovídající tomuto virtuálnímu počítači.
4. Vytvořte trezor služby Recovery Services a ke konfiguraci zálohování migrovaného virtuálního počítače použijte akci **Zálohování** v horní části řídicího panelu trezoru. Podrobnosti o zálohování virtuálních počítačů do trezoru služby Recovery Services naleznete v tématu [První pohled: Ochrana virtuálních počítačů s trezorem Recovery Services](../articles/backup/backup-azure-vms-first-look-arm.md).

## <a name="can-i-validate-my-subscription-or-resources-to-see-if-theyre-capable-of-migration"></a>Je možné ověřit, jestli jsou prostředky nebo předplatné schopné migrace? 

Ano. U možnosti migrace s podporou platformy je prvním krokem přípravy na migraci ověření, že jsou prostředky schopné migrace. V případě selhání operace ověření se zobrazí zprávy se všemi důvody, proč migraci není možné dokončit.

## <a name="what-happens-if-i-run-into-a-quota-error-while-preparing-the-iaas-resources-for-migration"></a>Co se stane, pokud při přípravě prostředků IaaS na migraci dojde k chybě kvóty? 

Doporučujeme migraci přerušit a pak odeslat žádost o navýšení kvót v oblasti, ve které virtuální počítače migrujete. Po schválení žádosti o kvóty můžete znovu začít provádět kroky migrace.

## <a name="how-do-i-report-an-issue"></a>Jak se dá nahlásit problém? 

Problémy a dotazy týkající se migrace zveřejněte na našem [fóru k virtuálním počítačům](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows) s použitím klíčového slova ClassicIaaSMigration. Doporučujeme, abyste všechny vaše dotazy zveřejnili na tomto fóru. Pokud máte smlouvu o podpoře, můžete také odeslat lístek podpory.

## <a name="what-if-i-dont-like-the-names-of-the-resources-that-the-platform-chose-during-migration"></a>Co když se mi nelíbí názvy prostředků, které platforma zvolila během migrace? 

Všechny prostředky, pro které jste explicitně zadali název v modelu nasazení Classic, se během migrace zachovají. V některých případech se vytvoří nové prostředky. Příklad: pro každý virtuální počítač se vytvoří síťové rozhraní. Aktuálně nepodporujeme možnost upravovat názvy těchto nových prostředků vytvořených během migrace. Hlasujte pro tuto funkci na [fóru Azure pro názory](http://feedback.azure.com).

## <a name="can-i-migrate-expressroute-circuits-used-across-subscriptions-with-authorization-links"></a>Je možné migrovat okruhy ExpressRoute používané napříč předplatnými pomocí autorizačních odkazů? 

Okruhy ExpressRoute používající autorizační odkazy mezi předplatnými není možné automaticky migrovat bez výpadku. Nabízíme doprovodné materiály popisující ruční postup jejich migrace. Postup a další informace najdete v článku [Migrace okruhů ExpressRoute a přidružených virtuálních sítí z modelu nasazení Classic na Resource Manager](../articles/expressroute/expressroute-migration-classic-resource-manager.md).

## <a name="i-got-a-message-vm-is-reporting-the-overall-agent-status-as-not-ready-hence-the-vm-cannot-be-migrated-ensure-that-the-vm-agent-is-reporting-overall-agent-status-as-ready-or-vm-contains-extension-whose-status-is-not-being-reported-from-the-vm-hence-this-vm-cannot-be-migrated-"></a>Zobrazila se zpráva *„Virtuální počítač hlásí celkový stav agenta jako Není připraven. Virtuální počítač proto není možné migrovat. Ujistěte se, že agent virtuálního počítače hlásí celkový stav agenta jako Připraven“* nebo *„Virtuální počítač obsahuje rozšíření, jehož stav virtuální počítač nehlásí. Virtuální počítač proto není možné migrovat.“ *

Tato zpráva se zobrazí v případě, že virtuální počítač nemá odchozí připojení k internetu. Agent virtuálního počítače se pomocí odchozího připojení spojuje s účtem služby Azure Storage, aby každých pět minut aktualizoval stav agenta.
