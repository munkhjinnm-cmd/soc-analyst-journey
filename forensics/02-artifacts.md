# Windows Artifact Analysis — WIN-EP01

## 1. Зорилго
Windows Server 2022 системийн Prefetch, Scheduled Tasks, Registry Run болон хэрэглэгчийн бүртгэлийг шинжилж, өмнөх лабораторид үүсгэсэн `backdoor` хэрэглэгч болон persistence механизмын ул мөрийг тодорхойлох.

## 2. Prefetch шинжилгээ

**Команд:**
```powershell
Get-ChildItem "C:\Windows\Prefetch\*.pf" -ErrorAction SilentlyContinue |
Sort-Object LastWriteTime -Descending |
Select-Object Name, LastWriteTime -First 20
```

**Үр дүн:**
- `C:\Windows\Prefetch` хавтас байгаа.
- `.pf` файлын жагсаалт харагдаагүй.
- Иймээс Prefetch артефактаар өмнө ажилласан программуудыг баталгаажуулах боломжгүй байв.

## 3. Scheduled Tasks шинжилгээ

**Команд:**
```powershell
Get-ScheduledTask |
Where-Object {$_.State -ne 'Disabled'} |
Select-Object TaskName, TaskPath, State
```

**Үр дүн:**
- Windows-ийн стандарт Scheduled Task-ууд илэрсэн.
- `backdoor` нэртэй task илрээгүй.
- `cmd.exe`, `cscript.exe` ашигладаг стандарт task-ууд байсан тул нэмэлт шалгалт хийсэн.
- Нэмэлт хайлтаар Windows Input-тэй холбоотой 4 task илэрсэн. Эдгээр нь `Available` гэсэн үгийн хэсэгтэй хайлтын нөхцөл таарснаас үр дүнд орсон.

## 4. Registry Run шинжилгээ

**Команд:**
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
```

**Үр дүн:**

| Name | Path | Үнэлгээ |
|---|---|---|
| SecurityHealth | C:\Windows\system32\SecurityHealthSystray.exe | Windows Security-ийн стандарт бүртгэл |

Шалгасан Registry Run түлхүүрт сэжигтэй startup бүртгэл илрээгүй.

## 5. Backdoor хэрэглэгчийн шинжилгээ

**Команд:**
```powershell
net user backdoor
```

**Үр дүн:**

| Үзүүлэлт | Утга |
|---|---|
| Username | backdoor |
| Account active | Yes |
| Account expires | Never |
| Password last set | 2026-10-28 00:43:32 |
| Last logon | Never |
| Local Group | Users |
| Administrators membership | Илрээгүй |

Өмнөх лабораторид үүсгэсэн `backdoor` хэрэглэгч системд идэвхтэй хадгалагдсан байна. Гэхдээ одоогийн шалгалтаар администраторын эрхгүй бөгөөд амжилттай нэвтэрсэн бүртгэл харагдахгүй байна.

## 6. Нэгдсэн дүгнэлт

WIN-EP01 серверийн артефактуудыг шалгахад `backdoor` хэрэглэгч идэвхтэй хэвээр байгаа нь тогтоогдсон. Харин Prefetch, Scheduled Tasks болон шалгасан Registry Run түлхүүрээс түүнтэй холбоотой persistence механизм батлагдсангүй.

Энэ нь өмнөх лабораторийн хэрэглэгчийн бүртгэлийн ул мөр хадгалагдсаныг харуулж байгаа боловч бодит халдлага явагдсан гэж дангаар нь нотлохгүй.

**Зөвлөмж:** Лабораторийн нотлох баримтыг хадгалсны дараа `backdoor` бүртгэлийг идэвхгүй болгох, Security Event Log болон Task Scheduler-ийн түүхийг нэмэлтээр шалгах.
