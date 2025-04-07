# Incident Response

## Lecture Notes: Incident Response

### What is an Incident?

* a violation or imminent threat of violation of computer security policies, acceptable use policies, or standard security practices
* adverse events that are computer security related, such as system crashes, packet floods, unauthorized use of system privileges, unauthorized access to sensitive data, and execution of malware that destroys data

### Incident Handling

* an action or plan for dealing with the misuse of computer systems and networks
* a coordinated and structured approach that goes from incident detection to resolution

### Goals of IR

* confirm whether or not an incident occurred
* provide rapid detection and containment
* determine and document the scope of the incident
* prevent a disjointed, non-cohesive response
* determine and promote facts and actual information
* minimize disruption to business and network operations
* minimize the damage to the compromised organization
* restore normal operations
* manage the public perception of the incident
* allow for criminal or civil actions against perpetrators
* educate senior management
* enhance the security posture of a compromised entity against further incidents

### Dwell Time

* the number of days from first evidence of compromise that an attacker is present on a victim network before detection
* median dwell time has gone from 16 days in 2022 to 10 days in 2023

### Motives

* nuisance
  * objective: access and propagation
  * example: botnets and spam
  * not targeted
  * often automated
* data theft
  * economic, political advantage
  * example: APTs
  * targeted
  * persistent
* cyber crime
  * objective: financial gain
  * example: credit card theft
  * targeted
  * frequently opportunistic
* hacktivism
  * objective: defamation, press and policy
  * example: website defacements
  * targeted
  * conspicuous
* destructive attack

### Attack Lifecycle

* initial recon
  * public profiles, job sites, DNS probing, public websites
* initial compromise
  * phishing
  * web browsing
* establish foothold
  * custom malware, keyloggers
* escalate privileges -> internal recon -> move laterally -> maintain presence
  * password cracking, application exploitation, dump password hashes, discover internal networks, plant backdoors, VPN subversion
* complete mission
  * steal/alter/destroy data, attack other networks

### IR Process

* preparation
* identification
* containment
* eradication
* recovery
* lessons learned

### IR Investigation Lifecycle

* initial leads start the process
* IOC creation -> deploy IOCs -> identify systems of interest -> collect evidence -> analyze data
  * iterative process

### IR Team Constituents

* core investigative team
* compliance
* legal
* HR
* network infrastructure
* business line managers
* desktop IT support

### Additional Notes

* M-trends report published by Mandiant discusses incident statistics
