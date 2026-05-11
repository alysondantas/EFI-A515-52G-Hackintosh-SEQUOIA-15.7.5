# EFI Acer Aspire A515-52G - macOS Sequoia 15.7.5

## Configuração

| Componente | Modelo |
|-----------|--------|
| **Processador** | Intel Core i5-8250U / i5-8265U (Kaby Lake R / Whiskey Lake) |
| **iGPU** | Intel UHD Graphics 620 |
| **dGPU** | Nvidia GeForce MX130 / MX150 (não suportada) |
| **Áudio** | Realtek ALC255 (layout-id 96) |
| **Ethernet** | Realtek RTL8111 |
| **WiFi/BT** | Intel Wireless-AC 3168 / 9560 |
| **Touchpad** | ELAN I2C |
| **EC** | Embedded Controller |

## Status

### Funcionando
- [x] Áudio (Alto-falante, microfone, headphone jack)
- [x] GPU Intel UHD 620 (com aceleração gráfica)
- [x] Gerenciamento de Energia (CPUFriend + VirtualSMC)
- [x] Brilho (teclas de atalho)
- [x] Ethernet (RealtekRTL8111)
- [x] Bluetooth Intel
- [x] Touchpad I2C + Teclado PS2
- [x] WebCam
- [x] USB (mapeado)
- [x] Leitor de cartão SD
- [x] Bateria (SMCBatteryManager)
- [x] Sleep/Wake

### Não funciona / Limitado
- [ ] Nvidia GTX (sem suporte em Hackintosh)
- [ ] HDMI externo (roteado pela Nvidia)
- [ ] WiFi Intel nativo (usa itlwm + HeliPort)

### WiFi Intel

O WiFi Intel **não funciona com AirportItlwm no Sequoia** (Apple removeu APIs necessárias). Solução:

1. Use **itlwm.kext** (incluído nesta EFI) + **HeliPort.app**
2. Baixe o HeliPort: [OpenIntelWireless/HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases)
3. Instale o `.app` no macOS e conecte-se às redes

## OpenCore

| Componente | Versão |
|-----------|--------|
| OpenCore | 1.0.7 |
| Lilu | 1.7.2 |
| WhateverGreen | 1.7.0 |
| AppleALC | 1.9.7 |
| VirtualSMC | 1.3.7 |
| CPUFriend | 1.3.0 |
| VoodooI2C | 2.9.1 |
| VoodooPS2 | 2.3.7 |
| RealtekRTL8111 | 3.0.0 |
| itlwm | 2.1.0 |
| IntelBluetoothFirmware | 2.4.0 |

## BIOS Settings

| Setting | Value |
|---------|-------|
| Main → Network Boot | Disable |
| Main → Wake on LAN | Disable |
| Main → Touchpad | Basic |
| Main → Lid Open Resume | Enable |
| Main → Wake on USB while lid closed | Disable |
| Main → D2D Recovery | Disable |
| Main → Fast boot | Disable |
| Advanced → Intel VTX | Disable |
| Advanced → Intel VID | Disable |
| Security → Set Supervisor Password | *Set a password* |
| Boot → Boot Mode | UEFI |
| Boot → Secure Boot | Disable |

## Debug

Esta EFI tem debug habilitado para diagnóstico:

- `AppleDebug=true` - logs do OpenCore
- `ApplePanic=true` - captura panics do kernel
- `Target=67` - logging completo
- `-v` modo verbose

Se o sistema reiniciar ao invés de mostrar o erro, as mensagens de panic são salvas na partição EFI ou em `/Library/Logs/DiagnosticReports/`.

## Pós-instalação

```bash
# Desabilitar hibernação
sudo pmset -a hibernatemode 0
sudo rm /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
sudo pmset -a standby 0
sudo pmset -a autopoweroff 0

# Habilitar TRIM (se SSD)
sudo trimforce enable
```

## Créditos

- [Acidanthera](https://github.com/acidanthera) - OpenCore e kexts
- [OpenIntelWireless](https://github.com/OpenIntelWireless) - WiFi e Bluetooth Intel
- [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C) - Touchpad
- [Mieze](https://github.com/Mieze/RTL8111_driver_for_OS_X) - Ethernet
