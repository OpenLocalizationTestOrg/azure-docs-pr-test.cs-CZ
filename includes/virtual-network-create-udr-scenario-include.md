## <a name="scenario"></a>Scénář
toobetter znázorňují toocreate udr tohoto dokumentu bude použití hello scénář níže.

![POPISEK OBRÁZKU](./media/virtual-network-create-udr-scenario-include/figure1.png)

V tomto scénáři vytvoříte jeden UDR pro hello *podsítě Front end* a jiné UDR pro hello *podsítě Back end* , jak je popsáno níže: 

* **UDR front-endu**. Hello front-endu UDR bude použité toohello *front-endu* podsítě a obsahovat jeden postup:    
  * **RouteToBackend**. Tato trasa bude odesílat všechny přenosy toohello back-end podsíť toohello **FW1** virtuálního počítače.
* **UDR back-end**. Hello back-end UDR bude použité toohello *back-end* podsítě a obsahovat jeden postup:    
  * **RouteToFrontend**. Tato trasa bude odesílat všechny přenosy toohello front-endu podsíť toohello **FW1** virtuálního počítače.

kombinace Hello tyto trasy zajistí, že se veškerý provoz, jehož z jedné podsítě tooanother budou směrované toohello **FW1** virtuální počítač, který je používán jako virtuální zařízení. Musíte taky tooturn na předávání IP pro tento virtuální počítač, tooensure mohl přijímat přenosy určené tooother virtuálních počítačů.

