# MacOS Tahoe 26.2 5600G + Asus Prime B450M-A II | OpenCore 1.0.6

**NOTA:**
> [!IMPORTANT]
> Esta EFI se realizó usando **OpenCore Auxiliary Tools (OCAT)**.
* **OpenCore 1.0.6**
* Sistema estable en **MacOS Tahoe** y posiblemente Sequioa.
* Verbose activado (No muestra logo de Apple), puedes desactivarlo quitando `-v` en `boot-args` para mostrarlo.

* **Considerar:** Para Sonoma/Sequoia, se recomienda desactivar `AMFIPass.kext`, limpiar boot-args de Tahoe y ajustar el sip en `csr-active-config` a ``00000000`` (valor original).
* **Activar Trim en MacOS:** Mejora rendimiento y evita desgaste de la vida útil del disco. Comando en terminal: ``sudo trimforce enable``
---

[![Pic-1.png](https://i.postimg.cc/021KHptD/Pic-1.png)](https://postimg.cc/23T51BNS)
[![Pic_2.png](https://i.postimg.cc/W4yhmrFd/Pic_2.png)](https://postimg.cc/B8xS4jC3)

## Especificaciones de Hardware
* **CPU:** AMD Ryzen 5 5600G (Cezanne)
* **Motherboard:** ASUS Prime B450M-A II (Última versión de BIOS)
* **GPU:** AMD Radeon Vega Graphics (Integrada)
* **RAM:** 16 GB (2x 8GB) DDR4 3000 MHz
* **Almacenamiento:** SUV400S37120G (KINGSTON SSD 120 GB)
* **Audio:** Realtek ALC887
* **Ethernet:** Realtek RTL8111

---
### ✅ Qué funciona:
* Aceleración gráfica (iGPU mediante NootedRed).
* Ethernet (Internet por cable).
* Gestión de energía y reposo.
* Puertos USB traseros.
* Audio Entrada y Salida (**con parche**)
* App Store (con seriales propios).

### ❌ No funciona / Limitaciones:
* **iMessage:** Es necesario serial original, lo cual no es víable usar por riesgo de baneo de icloud.
* **Audio Nativo en Tahoe:** Requiere parche manual desde **MacOS 26.2**.
* **WiFi / Bluetooth:** No incluidos / añadir kexts según el hardware.
* **DRM:** Streaming en Safari (Limitaciones de iGPU AMD en contenido protegido).

> [!CAUTION]
> **Chromium & NootedRed:**
> 
> Navegadores y aplicaciones basados en Chromium como Chrome, Edge o Brave presentan inestabilidad por conflictos de aceleración con la iGPU.
> 
* **Solución:** Deshabilitar la aceleración por hardware de la aplicación mediante Terminal.
* **Más información:** [chefkiss.dev](https://chefkiss.dev/applehax/nootedred/#chrome-chromium-based-browsers-and-apps-like-sublime-text-cause-graphical-artefacts-amongst-other-problems)

---

## Preinstalación
Ajustar los siguientes valores en **BIOS** para arranque correcto:

* Activar:
  - Above 4G Decoding
  - EHCI/XHCI Hand-off
  - AMD-V (SVM Mode).
  - UMA Framebuffer Size: **2G** (Mínimo 1G).
  
* Desactivar:
  - Fast Boot
  - Secure Boot
  - CSM.
  - Serial Port

 **Generar propios SMBIOS:**
1. Instala [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS).
2. Seleccionar el archivo `config.plist`.
3. Elegir Systemproductname (**de preferencia iMac20,1**), generar.
4. Generar ROM propia (no aleatoria)

**Mapeo de Puertos USB**
* La EFI Incluye `USBMap.kext` para **ASUS B450M-A II**, cubriendo todos los puertos traseros.
* No todos los puertos podrían funcionar. Se recomienda generar un mapeo propio con [`USBToolBox`](https://github.com/USBToolBox/tool) o intentar el quirk `XhciPortLimit`.

---

## Posinstalación
> [!WARNING]
> **El audio no funciona de manera nativa en macOS 26.2 Tahoe debido a cambios recientes por parte de Apple.**

* La EFI ya incluye: `AppleALC.kext`, `AMFIPass.kext` y el `layout-id 11`, pero desde MacOS 26.2 también se necesita un procedimiento más.
 **Cómo solucionar:**
> Leer [esta guía](https://github.com/Mirone/MyKextInstaller) para el método completo.

* **Versiones anteriores:** No requieren la solución. Sequoia o Sonoma es nativo con `layout-id 11`.
* **Alternativas:** En caso de no funcionar, probar otros ID: `1, 3, 13`.

---

## NVRAM / Seguridad
* **Para macOS Tahoe 26.2+:** Utilizar `030A0000` en `csr-active-config` para el parche de audio.
* **Otras versiones:** Se puede revertir a `00000000` (SIP habilitado).
* **Boot-args:** Incluye `-amfipassbeta` e `ipc_control_port_options=0`.

---
# Créditos
* [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) - OpenCore Install Guide.
* [corpnewt](https://github.com/corpnewt/GenSMBIOS) - GenSMBIOS
* [Acidanthera](https://github.com/acidanthera) - OpenCore, AppleALC, Lilu & VirtualSMC.
* [ic005k](https://github.com/ic005k/OCAuxiliaryTools) - OCAT (OpenCore Auxiliary Tools).
* [ChefKiss](https://chefkiss.dev/applehax/nootedred/) - NootedRed (AMD Vega iGPU Support).
* [MyKextInstaller](https://github.com/Mirone/MyKextInstaller) & KDK - AppleHDA Patching Tools.
* [trulyspinach](https://github.com/trulyspinach/SMCAMDProcessor) - SMCAMDProcessor.
* [AMD OS X](https://github.com/AMD-OSX/AMD_Vanilla) - AMD Vanilla Patches.
