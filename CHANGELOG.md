# Changelog

## v2.0 — 2026-06-06

Первый релиз **node-accelerator**. Заменяет `remnawave-node-toolkit` v1, исправляя
её баги и расширяя функциональность.

### ⚡ Оптимизатор (`optimize.sh`)
- **XanMod-ядро (BBRv3)** — авто-выбор сборки по psABI-уровню CPU, авто-skip на
  контейнерах (OpenVZ/LXC) и не-x86_64.
- **RPS/RFS/XPS** — раскидывает обработку пакетов по ядрам (критично на virtio-VPS).
- BBR + `fq`, буферы до 64 МБ, conntrack 2M, syncookies, anti-spoof `rp_filter=2`.
- nofile/nproc → 1M, swap, journald-cap, THP off, governor=performance, NIC tune.

### 🛡 Защита (`protect.sh`)
- **Исправлен баг порядка правил** v1: глобальный `syn accept` затенял весь
  port-allow-list, SSH-бан и portscan-детект. Теперь SYN-rate **per-IP**.
- **per-IP лимиты** (а не глобальные) — масштабируются по числу клиентов.
- **Полный IPv6-паритет** (в v1 v6-сканеры/брут не банились вообще).
- **Не `flush ruleset`** — своя таблица `inet na_filter`, сосуществует с
  CrowdSec-bouncer и Docker (v1 ломала Docker-сеть).
- AntiScan, flag-drop (XMAS/NULL/SYN+FIN/…), anti-spoof, SYN+UDP-flood, ssh-flood,
  `ct count` connlimit, rate-limit на логи.
- **CrowdSec + nftables firewall-bouncer** — поведенческий IPS + community-блоклист.

### 🩺 Диагностика (`diagnose.sh`)
- Read-only отчёт: ядро/BBR, sysctl, conntrack, NIC/RPS, firewall, CrowdSec, RTT.
