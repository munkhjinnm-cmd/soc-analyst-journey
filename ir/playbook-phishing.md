# Phishing Incident Response Playbook

## 1. Trigger
- Хэрэглэгч сэжигтэй имэйл мэдээлэх
- Mail gateway эсвэл SIEM phishing alert үүсгэх

## 2. Detection & Analysis
- [ ] Имэйлийн From, Return-Path, Reply-To, Subject, Received болон sender IP шалгах
- [ ] SPF, DKIM, DMARC authentication үр дүнг шалгах
- [ ] URLScan.io ашиглан URL, redirect, screenshot, HTTP requests шалгах
- [ ] WHOIS/RDAP ашиглан домэйны бүртгэлийн огноо, нас шалгах
- [ ] Хавсралтын төрөл, SHA256 hash, strings шалгах
- [ ] VirusTotal дээр hash-ийг хайх; нууц файл upload хийхгүй
- [ ] Public sandbox report-оос process tree, network, file, registry, ATT&CK шалгах
- [ ] IOC enrichment хийх, FP/TP/BTP эсвэл Undetermined verdict тогтоох

## 3. Scoping
- [ ] Имэйл хэдэн хэрэглэгчид хүрснийг mail logs-оор тогтоох
- [ ] Хэн имэйлийг нээсэн, холбоос дарсан, хавсралт ажиллуулсныг шалгах
- [ ] Холбогдох endpoint, DNS, proxy, EDR, SIEM лог шалгах
- [ ] Нөлөөлсөн хэрэглэгч, төхөөрөмж, хугацааг бүртгэх

## 4. Containment
- [ ] Батлагдсан хортой имэйлийг quarantine хийх эсвэл бүх inbox-оос устгах
- [ ] Батлагдсан хортой URL, домэйн, IOC-ийг холбогдох хамгаалалтын системд блоклох
- [ ] Халдварласан байж болзошгүй endpoint-ийг шалгаж, шаардлагатай бол тусгаарлах
- [ ] Tier 2 / Incident Response багт escalation хийх

## 5. Eradication & Recovery
- [ ] Account compromise батлагдвал password reset, session revoke, MFA шалгах
- [ ] Malware батлагдвал зөвшөөрөгдсөн IR журмаар цэвэрлэх
- [ ] Endpoint болон account-ийн хэвийн ажиллагааг баталгаажуулах
- [ ] Хэрэглэгчид мэдэгдэж phishing awareness зөвлөмж өгөх

## 6. Documentation
- [ ] Original header, screenshot, URLScan, VT, sandbox evidence хадгалах
- [ ] IOC жагсаалт болон enrichment үр дүнг бүртгэх
- [ ] Scoping, timeline, verdict, severity, escalation тэмдэглэх
- [ ] Хийсэн болон зөвхөн санал болгосон ажиллагааг ялгаж бичих
- [ ] GitHub Issue-д triage ticket үүсгэж холбоос хавсаргах

## 7. Training Case — GitHub Issue #8
- Case: Suspected Bank-Themed Phishing — Training Triage
- From: newsletter@securebankalerts.com
- Reply-To: support@bank-update-check.com
- SPF: FAIL
- DKIM: NONE
- DMARC: FAIL
- IOC candidates: securebankalerts.com, bank-update-check.com
- Verdict: Suspected Phishing (Training)
- Severity: Medium (training assessment)
- Recipients / clicks / compromised hosts: Unknown
- Containment / eradication: Not performed (training case)
- Ticket: https://github.com/munkhjinmm-cmd/soc-analyst-journey/issues/8

## 8. Conclusion
Сургалтын phishing кейсийн header шинжилгээнд сэжигтэй шинжүүд илэрсэн. Бодит хэрэглэгч, endpoint, compromise болон containment-ийн нотолгоо байхгүй тул хортой халдлага амжилттай болсон гэж дүгнээгүй.
