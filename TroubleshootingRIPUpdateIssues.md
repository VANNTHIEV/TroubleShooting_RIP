
# 🛠 Troubleshooting RIP Update Issues

## 1. **Check RIP Configuration**
- ពិនិត្យថា Router បាន enable **RIP (router rip)**
- បញ្ចូល **network command** ត្រឹមត្រូវ (ឧ. `network 192.168.1.0`)
- ពិនិត្យថា subnet/address ដែលបានកំណត់ត្រូវជាមួយ **interface**

👉 កំហុសធម្មតា៖ ព្យាយាម advertise network មិនត្រូវ (ex: /24 ជំនួស /30)

---

## 2. **Verify RIP Version**
- RIP មាន **Version 1** និង **Version 2**
  - **RIP v1** = classful (មិន support subnet mask)
  - **RIP v2** = classless (support VLSM និង subnet mask)
- ពិនិត្យប្រើ Command:  
  ```bash
  show ip protocols
  show running-config
  ```

👉 បញ្ហាធម្មតា៖ Router មួយប្រើ RIP v1, Router មួយទៀតប្រើ RIP v2 → update មិនចូលគ្នា។

---

## 3. **Check Interfaces**
- ត្រូវប្រាកដថា **interface** ដែល run RIP **up/up**
- Command:
  ```bash
  show ip interface brief
  ```
👉 ប្រសិនបើ interface down → RIP update មិនបានផ្ញើ។

---

## 4. **Check for Passive Interfaces**
- ប្រសិនបើបានប្រើ `passive-interface` → Router មិនផ្ញើ RIP update លើ interface នោះ។
👉 សាកល្បង `no passive-interface <interface>` ប្រសិនបើចង់ផ្ញើ update។

---

## 5. **Authentication Issues (RIP v2)**
- RIP v2 អាចប្រើ **authentication** (MD5 ឬ plaintext)
- ប្រសិនបើ keychain ខុស → Router មិនទទួល update។
👉 ពិនិត្យដោយ command:
```bash
show ip rip database
debug ip rip
```

---

## 6. **Check for Split Horizon**
- Split Horizon → Router មិនអាច re-advertise route ត្រឡប់ទៅ interface ដើម។
- ប្រសិនបើប្រើ **hub-and-spoke topology** → ត្រូវប្រើ `no ip split-horizon rip`។

---

## 7. **Debug RIP Updates**
- ប្រើ command ដើម្បីមើល packet RIP៖
  ```bash
  debug ip rip
  ```
👉 នេះអាចបង្ហាញថា update ត្រូវបានផ្ញើ ឬទទួលត្រឹមត្រូវឬអត់។

---

## 8. **Check for Route Filtering**
- ប្រសិនបើមាន **distribute-list** ឬ **route filtering** → RIP updates អាចត្រូវបាន block។
- ពិនិត្យ៖
```bash
show running-config
```

---

## 📌 សង្ខេប
ពេល troubleshooting RIP update issues ត្រូវពិនិត្យ៖  
1. Configuration (network statement, version)  
2. Interface status  
3. Passive-interface  
4. Authentication (RIP v2)  
5. Split Horizon  
6. Filtering (ACL/distribute-list)  
7. Debug logs  
