# نموكم — Nomucom Web App

Next.js 14 (App Router) + TypeScript + Tailwind CSS + Supabase.

## بنية المشروع

```
src/app/                 صفحات الموقع (App Router)
  page.tsx                الصفحة الرئيسية
  about/page.tsx           عن نموكم
  services/[slug]/page.tsx تفاصيل خدمة (ديناميكي)
  team/[slug]/page.tsx      تفاصيل عضو فريق (ديناميكي)
  booking/page.tsx          تدفق الحجز (4 خطوات)
  community/join/page.tsx   الانضمام للمجتمع
src/components/          مكونات مشتركة (Header, Footer, Cards)
src/lib/data.ts           بيانات الخدمات/البرامج/الكوتشات (مؤقتًا ثابتة)
src/lib/actions.ts        Server Actions - الكتابة الفعلية في Supabase
src/lib/supabase-client.ts عميل Supabase للمتصفح
supabase/schema.sql        مخطط قاعدة البيانات الكامل
public/                    الشعار، الفافيكون، صور الفريق (حقيقية)
```

## خطوات التشغيل محليًا

1. **تثبيت الحزم:**
   ```bash
   npm install
   ```

2. **إنشاء مشروع Supabase:**
   - أنشئ مشروعًا جديدًا على [supabase.com](https://supabase.com)
   - افتح SQL Editor وشغّل محتوى `supabase/schema.sql` بالكامل
   - من Storage، أنشئ bucket باسم `payment-proofs` (Public) لرفع إثباتات التحويل

3. **متغيرات البيئة:**
   - انسخ `.env.example` إلى `.env.local`
   - عبّي `NEXT_PUBLIC_SUPABASE_URL` و `NEXT_PUBLIC_SUPABASE_ANON_KEY` من
     Project Settings → API في Supabase

4. **تشغيل السيرفر المحلي:**
   ```bash
   npm run dev
   ```
   افتح http://localhost:3000

## ما هو حقيقي الآن وما زال Placeholder

**يعمل فعليًا (بعد ربط Supabase):**
- إنشاء حجز جديد (`createBooking`) يُدرج فعليًا في جدول `bookings`
- رفع إثبات التحويل فعليًا إلى Supabase Storage + جدول `payment_proofs`
- الانضمام لمجتمع نموكم يُدرج فعليًا في جدول `community_members`

**لسا Placeholder / يحتاج قرار من الفريق:**
- بيانات الخدمات والبرامج والكوتشات في `src/lib/data.ts` — ثابتة حاليًا،
  الخطوة التالية المنطقية نقلها لجداول Supabase (`services`, `programs`,
  `coaches`) بحيث تُدار من لوحة تحكم Admin بدل تعديل كود
- بيانات الحساب البنكي في خطوة الدفع - القيم الحقيقية (اسم البنك، رقم
  الحساب، IBAN) تُدخل لاحقًا في جدول `bank_account`
- لوحة تحكم Admin (Phase 8 في الخطة الأصلية) - لم تُبنَ بعد؛ الجداول
  والصلاحيات (RLS policies) جاهزة لها في `schema.sql`
- تسجيل الدخول (Supabase Auth) للعملاء والـ Admin - لم يُضف بعد
- صفحتا الخصوصية والشروط نصوص placeholder

## النشر (Deployment)

الطريقة الأسهل: [Vercel](https://vercel.com) (نفس الشركة المطوّرة لـ Next.js).

1. ارفع المشروع إلى GitHub
2. اربط المستودع بـ Vercel
3. أضف نفس متغيرات البيئة من `.env.local` في إعدادات المشروع على Vercel
4. Deploy

## الخطوات التالية المقترحة (بترتيب الأولوية)

1. نقل `services`/`programs`/`coaches` من كود ثابت إلى جداول Supabase
2. بناء صفحة تسجيل دخول Admin + Dashboard (عرض الحجوزات، تغيير الحالات)
3. تفعيل بوابة دفع إلكتروني (Moyasar/HyperPay/Tap) إذا تقرر التوسع لاحقًا
4. كتابة نصوص الخصوصية والشروط الفعلية
5. اختبار شامل على الجوال والديسكتوب قبل الإطلاق
