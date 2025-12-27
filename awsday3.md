# ✅ AWS Day-3: Subnetting (VPC & CIDR Explained)

🔹 Given VPC
VPC CIDR: 172.31.0.0/16
/16 → 16 bits for network
Remaining 16 bits for hosts
Total IPs in VPC = 2¹⁶ = 65,536

🔹 IPv4 Structure (32 bits)
IPv4 address = 32 bits

8 bits      8 bits      8 bits      8 bits
--------    --------    --------    --------
172         31          0           0

Binary view: 10101100.00011111.00000000.00000000

🔹 Subnet CIDR: /20
Subnet mask: /20
What does /20 mean?
20 bits → Network
12 bits → Hosts
32 - 20 = 12 host bits

🔹 Number of Hosts per /20 Subnet
2¹² = 4096 IPs
Usable hosts = 4096 - 2 = 4094
❌ Network IP
❌ Broadcast IP

🔹 Subnet Increment Calculation
Difference between:
/16 → /20 = 4 bits borrowed
2⁴ = 16
➡️ Subnet increment = 16 in the 3rd octet

🔹 Valid /20 Subnets inside 172.31.0.0/16
Subnet CIDR	Range Start	Range End
172.31.0.0/20	172.31.0.0	172.31.15.255
172.31.16.0/20	172.31.16.0	172.31.31.255
172.31.32.0/20	172.31.32.0	172.31.47.255
172.31.48.0/20	172.31.48.0	172.31.63.255
172.31.64.0/20	172.31.64.0	172.31.79.255
✅ 172.31.80.0/20	172.31.80.0	172.31.95.255
✅ 172.31.96.0/20	172.31.96.0	172.31.111.255

✔ 172.31.96.0/20 is VALID

🔹 Why 172.31.96.0/20 is Correct
Check increment:
0 → 16 → 32 → 48 → 64 → 80 → 96 ✔
Since 96 is divisible by 16, it’s a valid subnet boundary.

🔹 AWS Real-World Mapping
AWS Resource	Use
VPC	Big network (CIDR /16)
Subnet	AZ-level network (/20)
Route Table	Traffic control
IGW / NAT	Internet access
AWS commonly uses:
/16 for VPC
/20 or /24 for subnets

# 🎯 Interview Tips (Very Important)
/20 subnet = 4094 usable IPs
Subnet increment = 16
AWS reserves 5 IPs per subnet (extra exam point)

Always calculate increment in the octet where subnet mask stops
