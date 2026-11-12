# Reckless MS API — Cloudflare Workers، ملفات جذرية فقط

كل الملفات في جذر المستودع؛ لا يوجد مجلد فرعي:

```text
main.py
pyproject.toml
wrangler.jsonc
README.md
```

## ارفع الملفات إلى GitHub

من المستودع اختر **Add file → Upload files**، وارفع الملفات الأربعة أعلاه إلى الجذر، ثم اختر **Commit changes**. إذا كان عندك ZIP، فك ضغطه أولًا وارفع الملفات نفسها؛ لا ترفع ZIP بدل ملفات المشروع.

## اربط المستودع بـCloudflare Workers

اربط GitHub من **Workers & Pages → Create Worker / Workers Builds**، وليس كمشروع Cloudflare Pages. هذا مشروع Worker ديناميكي يعمل بـPython، وليس موقع Pages ثابتًا.

في إعدادات الـWorker افتح **Settings → Builds** واضبط:

- **Build command:** `uv sync --dev`
- **Deploy command:** `uv run pywrangler deploy`

احذف `npm run build`؛ المشروع لا يحتوي `package.json` ولا يحتاج npm build. سجل البناء الذي أرسلته يبيّن أن تثبيت Python نجح، ثم فشل فقط عند تنفيذ `npm run build`.

## إعداد ذاكرة KV والحماية

قبل أول نشر:

1. أنشئ KV namespace باسم `FACTS` في Cloudflare.
2. استبدل `REPLACE_WITH_KV_NAMESPACE_ID` في `wrangler.jsonc` بمعرّف KV الفعلي، واحفظ التغيير في GitHub.
3. من **Settings → Variables & Secrets** أضف secret باسم `RECKLESS_MS_API_KEY`.
4. أعد تشغيل Build/Deploy.

بعد نجاح النشر سيظهر رابط ينتهي بـ`workers.dev`. جرّب `/health` و`/docs`. المحادثة هي `POST /chat`، وتحليل الصور `POST /image`.

راجع [دليل Cloudflare Workers Builds](https://developers.cloudflare.com/workers/ci-cd/builds/configuration/) و[دليل Python Workers](https://developers.cloudflare.com/workers/languages/python/).
