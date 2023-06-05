## opnsense-suricata-nmaps
# OPNsense's Suricata IDS/IPS Detection Rules Against Nmap Scans
v. 1.0 / June 5th 2023 by Aleksi Bovellan

Because there weren't too many triggers against Nmap scans built-in OPNSense, or even in Suricata's ET Telemetry Pro ruleset, especially against different types of Nmap scans, I wrote a bundle of custom rules that react to even the slowest -T0 scans and fragmented ones too.

- 3 x Suricata rules to detect most Nmap scans WITHOUT port ranges. (Scan types include at least: -Pn, -sS, -sT, -sU, -A, -f)
- 3 x Suricata rules to detect most Nmap scans WITH more specific, common, or known port targets or ranges. (Scan types include at least: -Pn, -sS, -sT, -sU, -A, -f)
- Detected Nmap scan speeds: between modes -T5-T0. (Slowest Nmap scan detections take more time)
- Tested with no false alarms in personal / home / SoHo network environments


(If running both OPNSense/Suricata and CrowdSec, CrowdSec bans source IP addresses for Nmap scan speeds down to -T2, but not to -T1-T0. CrowdSec also ignores fragmented Nmap scans.)


NMAP EXAMPLES USING:   Nmap 7.9+ / Kali Linux 2023+	  VS.   OPNsense 23.1+  /  Suricata 6.0+  /  CrowdSec 1.5+  /  FreeBSD 13.1+

- nmap -Pn -sS -T1     ->     DETECTED BY SURICATA
- nmap -Pn -sS -f -T1     ->     DETECTED BY SURICATA
- nmap -Pn -sT -T1     ->     DETECTED BY SURICATA
- nmap -sT -f -T1     ->     DETECTED BY SURICATA
- nmap -Pn -sU -T1     ->     DETECTED BY SURICATA
- nmap -sU -f -T1     ->     DETECTED BY SURICATA
- nmap -Pn -sS -p 20-140 -T1     ->     DETECTED BY SURICATA
- nmap -Pn -sS -p 20-140 -f -T1     ->     DETECTED BY SURICATA
- nmap -Pn -sT -p 20-140 -T1     ->     DETECTED BY SURICATA
- nmap -sT -p 20-140 -f -T1     ->     DETECTED BY SURICATA
- nmap -Pn -sU -p 20-140 -T1     ->     DETECTED BY SURICATA
- nmap -sU -p 20-140 -f -T1     ->     DETECTED BY SURICATA
- nmap -Pn -sS -p 21,22,23,69,80,138,139,140 -T1     ->     DETECTED BY SURICATA
- nmap -Pn -sS -p 21,22,23,69,80,138,139,140 -f -T1     ->     DETECTED BY SURICATA
- nmap -Pn -sS -A -T0     ->     DETECTED BY SURICATA
- nmap -Pn -sS -p 21,22,23,69,80,138,139,140 -T0     ->     DETECTED BY SURICATA
- nmap -Pn -sS -p 21,22,23,69,80,138,139,140 -f -T0     ->     DETECTED BY SURICATA

- nmap -Pn -sU -T0     ->     NOT DETECTED BY SURICATA
- nmap -Pn -sU -f -T0     ->     NOT DETECTED BY SURICATA

# USAGE:

- Save the "local.rules" file, or write all alerts in it into ->  /usr/local/etc/suricata/rules/local.rules
- Reload OPNSense's Web GUI's "INTRUSION DETECTION" ‘RULES’ LIST + APPLY RULES
