


## <a name="tagging-a-virtual-machine-through-templates"></a>Označování virtuálního počítače prostřednictvím šablony
První Podíváme se na označování prostřednictvím šablon. [Tato šablona](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) umístí značky na hello následující prostředky: výpočetní (virtuálním počítači), úložiště (účet úložiště) a sítě (veřejnou IP adresu, virtuální sítě a síťové rozhraní). Tato šablona je pro virtuální počítač s Windows, ale můžete přizpůsobit pro virtuální počítače s Linuxem.

Klikněte na tlačítko hello **nasazení tooAzure** tlačítko z hello [šablony odkaz](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). To bude přejděte toohello [portál Azure](https://portal.azure.com/) kde můžete nasadit tuto šablonu.

![Jednoduché nasazení pomocí značek](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Tato šablona zahrnuje hello následující značky: *oddělení*, *aplikace*, a *vytvořil*. Vám může přidat či upravit tyto značky přímo v šabloně hello Pokud byste chtěli názvy různých značek.

![Azure značky v šabloně](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Jak vidíte, hello značky jsou definovány jako páry klíč/hodnota, oddělené dvojtečkou (:). Hello značky musí být definovány v tomto formátu:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Uložte soubor šablony hello po dokončení úprav s hello značkami, podle vašeho výběru.

Vedle hello **upravit parametry** části můžete vyplnit hello hodnoty pro značek.

![Úprava značek na portálu Azure](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Klikněte na tlačítko **vytvořit** toodeploy této šablony se vaše hodnoty značky.

## <a name="tagging-through-hello-portal"></a>Označování prostřednictvím hello portálu
Po vytvoření vaše prostředky pomocí značek, můžete zobrazit, přidat a odstranit značky hello portálu.

Vyberte hello značky ikonu tooview značek:

![Ikona značky na portálu Azure](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Přidejte novou značku prostřednictvím portálu hello tak, že definujete vlastní dvojice klíč/hodnota a uložte ho.

![Přidat novou značku na portálu Azure](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Novou značku by se měla zobrazit v seznamu hello značky prostředku.

![Novou značku uložit na portálu Azure](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

