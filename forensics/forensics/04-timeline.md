# D. Digital Forensics — Timeline Analysis

## 1. Зорилго

WIN-EP01 Windows Server дээр цуглуулсан Security Event Log, Sysmon, процесс, Prefetch, Scheduled Tasks болон Registry артефактуудыг нэгтгэн үйл явдлын цагийн дарааллыг тогтоох, `backdoor` хэрэглэгчтэй холбоотой үйл ажиллагааг шинжлэх.

## 2. Ашигласан орчин

| Үзүүлэлт | Мэдээлэл |
|---|---|
| Шинжилгээний систем | WIN-EP01 |
| Үйлдлийн систем | Windows Server 2022 |
| Хэрэглэгч | WIN-EP01\Administrator |
| Хэрэгсэл | PowerShell, Windows Event Log, Sysmon |
| Шинжилгээний огноо | 2026.10.28 |
| Цагийн бүс | UTC+8 |
| Үндсэн артефакт | Event ID 4720, 4726, Sysmon Event ID 1 |

## 3. Timeline — Үйл явдлын дараалал

| Цаг (UTC+8) | Артефакт | Үйл явдал | Эх үүсвэр |
|---|---|---|---|
| 00:45:35 | Process Create | `net.exe` болон `net1.exe` ашиглан `backdoor` хэрэглэгч үүсгэсэн | Sysmon Event 1 |
| 00:45:35 | Account Created | `backdoor` хэрэглэгч үүссэн | Security Event 4720 |
| 00:59:36 | Process Create | `backdoor` хэрэглэгчийг устгах команд ажилласан | Sysmon Event 1 |
| 00:59:36 | Account Deleted | `backdoor` хэрэглэгч устгагдсан | Security Event 4726 |
| 00:59:43 | Account Created | `backdoor` хэрэглэгч дахин үүссэн | Security Event 4720 |
| 01:29:42 | Account Deleted | `backdoor` хэрэглэгч дахин устгагдсан | Security Event 4726 |
| 01:43:32 | Process Create | `backdoor` хэрэглэгчийг дахин үүсгэх команд ажилласан | Sysmon Event 1 |
| 01:43:32 | Account Created | `backdoor` хэрэглэгч дахин үүссэн | Security Event 4720 |

**Тайлбар:** Sysmon-ийн `UtcTime` талбарын UTC+0 цагийг UTC+8 болгон хөрвүүлсэн. Хүснэгтэд секундийн түвшинд нэгтгэсэн бөгөөд зарим үйл явдлын яг цагийг Security Event Log-оос авсан.

## 4. Артефактуудын шинжилгээ

### 4.1. Windows Security Event Log

- **Event ID 4720:** Шинэ хэрэглэгчийн бүртгэл үүссэн.
- **Event ID 4726:** Хэрэглэгчийн бүртгэл устгагдсан.
- **Subject:** WIN-EP01\Administrator
- **Target Account:** backdoor

Эдгээр бүртгэл нь `backdoor` хэрэглэгчийг хэд хэдэн удаа үүсгэж, устгасан болохыг харуулж байна.

### 4.2. Sysmon Event ID 1

Sysmon бүртгэлээс дараах мэдээлэл илэрсэн.

- **Process:** `net.exe`, `net1.exe`
- **Parent Process:** `powershell.exe`, зарим тохиолдолд `net.exe`
- **User:** WIN-EP01\Administrator
- **Үйлдэл:** `net user backdoor ... /add`, `net user backdoor /delete`

Sysmon нь зөвхөн хэрэглэгч үүссэн эсэхийг бус, ямар процесс болон команд ашигласныг тогтооход тусалсан.

### 4.3. Prefetch

`C:\Windows\Prefetch` хавтас байгаа нь батлагдсан боловч `.pf` файлуудын жагсаалтад үр дүн гараагүй.

Иймээс Prefetch-ээр `net.exe` эсвэл `cmd.exe` ажилласан цагийг баталгаажуулах боломжгүй байсан.

### 4.4. Scheduled Tasks

Scheduled Tasks-ийг шалгахад Microsoft Edge Update, Windows Defender болон Windows-ийн бусад системийн task-ууд илэрсэн.

`backdoor` нэртэй task илрээгүй. Скрипт ажиллуулах боломжтой task-уудыг нэмэлтээр шалгасан боловч `backdoor`-той холбоотой persistence батлагдаагүй.

### 4.5. Registry Run

Шалгасан түлхүүр:

`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`

Илэрсэн утга:

`SecurityHealth = C:\Windows\system32\SecurityHealthSystray.exe`

Энэ түлхүүрт `backdoor` нэртэй autorun бүртгэл илрээгүй.

### 4.6. Одоогийн хэрэглэгчийн төлөв

`Get-LocalUser` болон `net user backdoor` командуудаар `backdoor` бүртгэл идэвхтэй байгааг тогтоосон.

Гэхдээ уг хэрэглэгч `Administrators` бүлэгт хамаарахгүй, зөвхөн `Users` бүлэгт байгааг шалгасан.

## 5. Үйл явдлын тайлбар

2026 оны 10 дугаар сарын 28-нд WIN-EP01 сервер дээр Administrator эрхээр PowerShell-ээс `net.exe` болон `net1.exe` командуудыг ажиллуулж, `backdoor` нэртэй хэрэглэгчийн бүртгэл үүсгэсэн байна. Уг хэрэглэгчийг дараа нь устгаж, дахин үүсгэх үйлдэл хэд хэдэн удаа давтагдсан нь Security Event 4720, 4726 болон Sysmon Event 1-ээр батлагдсан. Энэ үйл ажиллагаа нь системд дахин нэвтрэх боломж бүрдүүлэх оролдлоготой төстэй боловч лабораторийн туршилтын үйлдэл учраас бодит халдлага гэж шууд дүгнэхгүй. Нотлох баримтууд нь хэрэглэгчийн бүртгэлийн өөрчлөлт, ашигласан команд болон процессын хамаарлыг тодорхой харуулж байна.

## 6. Дүгнэлт

Windows Security Event Log болон Sysmon-ийн мэдээллийг нэгтгэснээр `backdoor` хэрэглэгчийг үүсгэх, устгах үйл явдлын дарааллыг тогтоосон.

Security Event Log нь хэрэглэгчийн бүртгэлийн өөрчлөлтийг, Sysmon нь тухайн үйлдлийг гүйцэтгэсэн процесс болон командын мэдээллийг өгсөн.

Prefetch, Scheduled Tasks болон Registry Run шалгалтаар нэмэлт persistence механизм батлагдаагүй.

**Verdict:** Лабораторийн хүрээнд үүсгэсэн, persistence-тэй төстэй хэрэглэгчийн бүртгэлийн үйл ажиллагаа.

**Status:** Timeline analysis completed.

## 7. Нотлох баримт

### Screenshot 1. Security Event ID 4720
<img width="707" height="651" alt="Screenshot 2026-11-04 182329" src="https://github.com/user-attachments/assets/8631417f-398c-4fdc-ad6c-98633b895dff" />
**Тайлбар:** Administrator хэрэглэгчээс `backdoor` нэртэй шинэ хэрэглэгч үүсгэсэн бүртгэл.

### Screenshot 2. Security Event ID 4726
<img width="727" height="290" alt="Screenshot 2026-11-04 182419" src="https://github.com/user-attachments/assets/e6d61396-ddc1-4d70-b42d-82655dbe6ba1" />
**Тайлбар:** `backdoor` хэрэглэгчийг устгасан үйлдлийн бүртгэл.

### Screenshot 3. Sysmon Event ID 1
<img width="971" height="447" alt="Screenshot 2026-11-04 182542" src="https://github.com/user-attachments/assets/b680445f-ab8e-4dd9-b0d4-66d52dbbdf2a" />
**Тайлбар:** `backdoor` хэрэглэгчийг үүсгэх, устгах командуудын процессын нотолгоо. Нууц үгийн хэсгийг халхалсан байх шаардлагатай.

### Screenshot 4. Prefetch, Scheduled Tasks, Registry
<img width="720" height="107" alt="Screenshot 2026-11-04 183046" src="https://github.com/user-attachments/assets/6e32ef40-314f-447f-8b57-aa982136206a" />
**Зураг 4.1. Prefetch шалгалт**
<img width="872" height="473" alt="Screenshot 2026-11-04 183303" src="https://github.com/user-attachments/assets/20a8221f-876c-4d8b-9a6f-14b5a6a76567" />
**Зураг 4.2. Scheduled Tasks шалгалт**
<img width="948" height="178" alt="Screenshot 2026-11-04 183430" src="https://github.com/user-attachments/assets/8ad1ba25-131c-40d6-a00f-64d28dc81298" />
**Зураг 4.3. Registry Run шалгалт**
**Тайлбар:** Prefetch хавтас байгаа боловч `.pf` файлын жагсаалт хоосон байсан. Scheduled Tasks шалгалтаар `backdoor`-той холбоотой task илрээгүй. Registry Run түлхүүрт `SecurityHealth` бүртгэл байсан бөгөөд `backdoor` autorun илрээгүй.
