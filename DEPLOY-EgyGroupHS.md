# نشر موقع Egyptian Group على السيرفر

## الخطوة 1 — رفع على GitHub (من جهازك، مرة واحدة)
بعد تنزيل EgyGroupHS.zip وفكّه:
```bash
git init
git add .
git commit -m "Egyptian Group Next.js site"
git branch -M main
git remote add origin https://github.com/AbdelrhmanAbdelkafy/EgyGroupHS.git
git push -u origin main
```
(GitHub هيطلب username + Personal Access Token)

## الخطوة 2 — على السيرفر: سحب + بناء
```bash
cd /root
git clone https://github.com/AbdelrhmanAbdelkafy/EgyGroupHS.git egygroup
cd egygroup
npm install
npm run build
```

## الخطوة 3 — تشغيل آمن على بورت اختبار (3100) بدون لمس Apache
```bash
nohup npx next start -p 3100 > /root/egygroup.log 2>&1 &
# اختبار:
curl -sI http://localhost:3100/ar | head -3
```
افتح: http://153.92.209.190:3100/ar  — لو ظهر تمام، كمّل.

## الخطوة 4 — استبدال الموقع القديم
- أوقف كونتينر front القديم (الواقع):
  `docker stop front && docker rm front`
- وجّه Apache من 3000/3001 → 3100 (في ملف الـ vhost)
- أو خلّي next start على نفس بورت الموقع القديم.

## الخطوة 5 — جعله دائم (systemd) بدل nohup
لاحقًا: نعمل systemd service عشان يشتغل تلقائيًا بعد أي restart.

## ملاحظات
- بيانات التواصل placeholders في src/content/site.ts — تتحدّث قبل الإطلاق.
- الصور فيها لوجوهات ماركات — تتشال قبل الإطلاق.
