# Oracle Cloud Optimization Package for BigMinimal

This package provides optimizations specifically designed for running BigMinimal Linux on Oracle Cloud Infrastructure (OCI) with AMD EPYC processors. It includes a collection of system optimizations to enhance performance, stability, and connectivity for cloud deployments.

## Overview

`comm-oracle-optimize` is tailored for servers running in Oracle Cloud environments, with specific optimizations for AMD EPYC processors. This package is NOT intended for desktop use or non-Oracle cloud environments, as the optimizations are specifically calibrated for Oracle's virtualization infrastructure.

## Features

- **TCP/IP Optimizations**
  - BBR congestion control algorithm implementation
  - Increased TCP buffer sizes for high throughput
  - Optimized network parameters for cloud virtualized environments
  - Fine-tuned TCP settings for reduced latency

- **System Resource Management**
  - Increased file descriptor limits (65536)
  - Optimized virtual memory parameters
  - Enhanced kernel resource allocation

- **Cloud Console Access**
  - Serial console configuration for remote troubleshooting
  - Guaranteed access even when network connectivity fails

- **AMD EPYC Specific Optimizations**
  - Memory management adjustments for EPYC architecture
  - NUMA balancing optimizations

## Installation

Install this package using your BigMinimal package manager:

```bash
sudo pacman -S comm-oracle-optimize
```

## Post-Installation

A system reboot is recommended after installation to ensure all optimizations are properly applied. The package will:

1. Apply sysctl optimizations immediately
2. Configure GRUB for serial console access
3. Set appropriate system limits
4. Ensure BBR congestion control is enabled

## Requirements

- BigMinimal Linux
- Oracle Cloud Infrastructure with AMD EPYC processors
- Administrative privileges

## Configuration Files

This package modifies or creates the following configuration files:

- `/etc/sysctl.d/99-oracle-optimize.conf`: Kernel parameter optimizations
- `/etc/security/limits.d/99-oracle-optimize.conf`: System resource limits
- `/etc/default/grub`: Adds serial console parameters

## Detailed Sysctl Optimizations

The package applies the following kernel parameter optimizations:

### Basic Kernel Settings
```
kernel.core_uses_pid = 1
kernel.panic = 3
kernel.printk = 4 4 1 7
kernel.sysrq = 0
kernel.panic_on_oops = 1
kernel.unknown_nmi_panic = 0
kernel.randomize_va_space = 2
kernel.numa_balancing = 1
kernel.sched_autogroup_enabled = 0
```

### IPv6 Disabling
```
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

### System Message Limits
```
kernel.msgmax = 65536
kernel.msgmnb = 65536
```

### Network Core Optimizations
```
net.core.somaxconn = 65535
net.core.netdev_max_backlog = 65536
net.core.rmem_default = 1048576
net.core.rmem_max = 26214400
net.core.wmem_default = 1048576
net.core.wmem_max = 26214400
net.core.optmem_max = 65536
```

### Network Security Settings
```
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.all.arp_ignore = 1
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.arp_ignore = 1
net.ipv4.conf.default.log_martians = 1
```

### ICMP Settings
```
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.ipv4.icmp_echo_ignore_all = 0
```

### TCP/IP Base Settings
```
net.ipv4.ip_default_ttl = 77
net.ipv4.ip_forward = 1
net.ipv4.ip_local_port_range = 1024 65535
net.ipv4.ip_no_pmtu_disc = 1
```

### Network Neighbor Settings
```
net.ipv4.neigh.default.gc_interval = 5
net.ipv4.neigh.default.gc_stale_time = 120
net.ipv4.neigh.default.gc_thresh1 = 4096
net.ipv4.neigh.default.gc_thresh2 = 8192
net.ipv4.neigh.default.gc_thresh3 = 16384
net.ipv4.route.flush = 1
```

### BBR Congestion Control
```
net.ipv4.tcp_congestion_control = bbr
```

### TCP Optimizations
```
net.ipv4.tcp_dsack = 1
net.ipv4.tcp_ecn = 0
net.ipv4.tcp_fack = 1
net.ipv4.tcp_fin_timeout = 10
net.ipv4.tcp_keepalive_intvl = 10
net.ipv4.tcp_keepalive_probes = 6
net.ipv4.tcp_keepalive_time = 60
net.ipv4.tcp_max_syn_backlog = 16384
net.ipv4.tcp_no_metrics_save = 1
net.ipv4.tcp_rfc1337 = 1
net.ipv4.tcp_sack = 1
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_syn_retries = 2
net.ipv4.tcp_synack_retries = 2
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_timestamps = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_low_latency = 1
```

### TCP/UDP Memory Buffers
```
net.ipv4.tcp_rmem = 4096 1048576 26214400
net.ipv4.tcp_wmem = 4096 1048576 26214400
net.ipv4.udp_rmem_min = 16384
net.ipv4.udp_wmem_min = 16384
net.ipv4.tcp_max_tw_buckets = 2000000
net.ipv4.tcp_mtu_probing = 1
```

### Virtual Memory Optimizations
```
vm.dirty_background_ratio = 5
vm.dirty_ratio = 15
vm.vfs_cache_pressure = 50
vm.swappiness = 10
vm.zone_reclaim_mode = 0
vm.max_map_count = 262144
```

## Backup and Recovery

During installation, the package creates backups of your original configuration files:

- `/etc/sysctl.conf.bak.YYYYMMDD`
- `/etc/security/limits.conf.bak.YYYYMMDD`
- `/etc/default/grub.bak.YYYYMMDD`

If you need to revert to original settings, these backup files can be restored manually.

## Security Considerations

This package modifies some security-related network settings to optimize for performance in trusted cloud environments. Review the changes if you have specific security requirements.

## Compatibility

- Designed specifically for **BigMinimal Linux**
- Optimized for **Oracle Cloud Infrastructure**
- Tuned for **AMD EPYC processors**
- Not recommended for other environments or processor architectures

## License

This package is distributed under the MIT License.

## Support

For issues, feature requests, or contributions, please visit the GitHub repository:
https://github.com/communitybig/comm-oracle-optimize

---

© 2025 BigCommunity Linux
