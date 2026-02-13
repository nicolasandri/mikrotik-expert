# MikroTik Expert Skill

![MikroTik](https://img.shields.io/badge/MikroTik-RouterOS-blue)
![Skill](https://img.shields.io/badge/Skill-TR--069-green)
![ISP](https://img.shields.io/badge/Use-ISP%2FWISP-orange)

Skill AI untuk troubleshooting dan manajemen RouterOS MikroTik, khusus untuk operasi ISP/WISP.

## 🎯 Fitur

- ✅ **PPPoE Troubleshooting** - Diagnosa koneksi PPPoE yang gagal
- ✅ **Bandwidth Diagnostics** - Cek kecepatan dan latensi
- ✅ **Hotspot Management** - Atasi masalah login hotspot
- ✅ **Firewall & NAT** - Troubleshooting firewall dan NAT
- ✅ **VLAN Configuration** - Verifikasi konfigurasi VLAN
- ✅ **Scripting Examples** - Script otomasi untuk monitoring
- ✅ **Security Checklist** - Best practices keamanan MikroTik

## 📦 Cara Install

### Via skills.sh (Claude Code, Cursor, dll)

```bash
npx skills add https://github.com/YOUR_USERNAME/mikrotik-expert --skill mikrotik-expert
```

### Manual Install

1. Copy folder `mikrotik-expert` ke project Anda:
```bash
cp -r mikrotik-expert ~/.skills/
```

2. Atau buat symlink:
```bash
ln -s $(pwd)/mikrotik-expert ~/.skills/mikrotik-expert
```

## 🚀 Penggunaan

Setelah install, AI agent akan otomatis mengenali skill ini saat Anda bertanya tentang:

- "Pelanggan tidak bisa konek internet"
- "PPPoE disconnected terus"
- "Hotspot login tidak bisa"
- "Bandwidth pelanggan lambat"
- "Firewall blocking koneksi"

## 📋 Workflow yang Tersedia

### 1. PPPoE Connection Troubleshooting
Diagnosa dan perbaiki koneksi PPPoE yang gagal:
- Check status PPPoE client
- Verifikasi kredensial
- Analisis log error
- Perbaiki MTU/MRU issues

### 2. Bandwidth/Latency Diagnostics
Identifikasi masalah kecepatan:
- Test bandwidth dengan tool
- Check packet loss
- Analisis queue tree
- Monitor CPU dan memory

### 3. Hotspot User Issues
Atasi masalah login hotspot:
- Check user authentication
- Reset counters
- Verifikasi walled-garden
- Perbaiki session expired

### 4. Firewall/NAT Troubleshooting
Perbaiki masalah firewall:
- Check NAT rules
- Verifikasi filter rules
- Analisis connection tracking
- Perbaiki rule order

### 5. VLAN Configuration Check
Verifikasi setup VLAN:
- Check VLAN interface
- Verifikasi bridge port
- Validasi VLAN tagging

## 📝 Quick Reference Commands

```bash
# Check PPPoE Status
/interface pppoe-client print
/interface pppoe-client monitor [find name=pppoe-out1]

# Test Connectivity
/ping 8.8.8.8 count=100
/tool bandwidth-test 8.8.8.8 direction=both duration=10s

# Check Logs
/log print where topics~"ppp"
/log print follow

# System Info
/system resource print
/system routerboard print
```

## 🔧 Common Issues

| Issue | Command | Solution |
|-------|---------|----------|
| PPPoE disconnected | `/interface pppoe-client print` | Check auth, MTU, keepalive |
| Slow internet | `/tool bandwidth-test` | Check queue, CPU, packet loss |
| Hotspot login fail | `/ip hotspot active print` | Reset counters, check walled-garden |
| NAT not working | `/ip firewall nat print` | Verify masquerade rule |

## 📚 Dokumentasi Lengkap

Lihat file [SKILL.md](./SKILL.md) untuk dokumentasi lengkap termasuk:
- Core workflows detail
- Scripting examples
- Best practices
- Security checklist

## 🤝 Kontribusi

Silakan buat pull request jika ingin menambahkan:
- Workflow baru
- Script otomasi
- Parameter tambahan
- Dokumentasi bahasa lain

## 📄 Lisensi

MIT License - Silakan gunakan untuk keperluan ISP/WISP Anda.

---

**Dibuat untuk memudahkan troubleshooting MikroTik di dunia ISP** 🚀
