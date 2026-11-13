# Дадлага — хавтасгүй хувилбар

Энэ багц дотор зөвхөн 8 файл байна. Бүх код, загвар, лого, фонт, эх PDF болон браузерын JS/CSS нь dadlaga.mjs файл дотор багтсан. package-lock.json нь хамаарлын хувилбаруудыг, OFL.txt нь фонтын лицензийг агуулна.

Сервер нь Cloudflare Workers, өгөгдлийн сан нь D1, хувийн хавсралтын хадгалалт нь R2 ашиглана. Админ, дадлагажигчийн нэвтрэлт, хяналт, хадгалалт болон PDF тайлангийн ажиллагааг хадгалсан. Браузерт харагдах загвар нь анхны хувилбартай ижил.

## GitHub-д оруулах

ZIP-ийг задлаад доторх 8 файлыг GitHub-ийн Add file → Upload files → Choose your files хэсгээс хамтад нь сонгоно. Хавтас сонгох шаардлагагүй. Ижил нэртэй package.json, package-lock.json, wrangler.jsonc, README.md файлуудыг энэ хувилбараар шинэчилнэ. Commit changes дарна. Өмнө оруулсан бусад эх файлууд байж болно; энэ багцын deploy нь dadlaga.mjs-ийг ашиглана.

## Cloudflare дээр ажиллуулах

1. Node.js 22 буюу түүнээс шинэ хувилбарт npm install ажиллуулна.
2. npx wrangler login гэж Cloudflare бүртгэлээрээ нэвтэрнэ.
3. Ашиглах D1 өгөгдлийн сан: dadlga (ee2adafb-0360-4235-9a58-7f03d4cad040). Энэ өгөгдлийн сан тохиргоонд холбогдсон; дараагийн алхамд хүснэгтүүдийг үүсгэнэ.
4. npx wrangler r2 bucket create dadlaga-files ажиллуулна. R2 идэвхжүүлсэн бүртгэл шаардлагатай.
5. npm run deploy ажиллуулна. Энэ команд эхлээд хүлээгдэж буй D1 migration-уудыг хэрэглэж хүснэгтүүдийг үүсгээд, дараа нь Worker-ийг нийтэлнэ. CI/CD орчинд Wrangler баталгаажуулалтын асуултыг автоматаар алгасана.
6. Cloudflare-ийн Worker → Settings → Variables and Secrets хэсэгт AUTH_SECRET, ADMIN_EMAIL, ADMIN_PASSWORD_HASH, SITE_URL утгуудыг тохируулна. ADMIN_EMAIL нь админы и-мэйл; SITE_URL нь нийтлэгдсэн сайтын үндсэн https холбоос байна (төгсгөлд / тэмдэггүй). AUTH_SECRET болон ADMIN_PASSWORD_HASH-ийг Secret төрлөөр оруулна.

Cloudflare-ийн GitHub холболтоор нийтлэхэд Build command нь npm run build, Deploy command нь npm run deploy байна. Ингэснээр өгөгдлийн сангийн schema нийтлэхээс өмнө хэрэглэгдэнэ.

Нууц тохиргоог эх кодод болон GitHub-д оруулахгүй. prepare-secrets.mjs туслахыг node prepare-secrets.mjs гэж ажиллуулаад админы нууц үгээ интерактив оруулахад adminHash, authSecret утгууд үүснэ. Эдгээрийг зөвхөн Worker-ийн Secret тохиргоонд хадгална. Нууц үг бичихэд терминалын тэмдэгт харагдах боломжтой тул хувийн терминалд ашиглана.

GitHub нь кодыг хадгална. Энэ нь Cloudflare Workers дээр ажиллах серверийн багц бөгөөд GitHub Pages эсвэл Netlify-ийн статик upload болгон шууд ажиллуулах тохиргоо агуулаагүй.

Noto Sans фонтуудын SIL Open Font License нь сайтын /assets/OFL.txt хаягт, эх дадлагын дэвтэр нь /assets/dadlagyn-devter.pdf хаягт агуулагдана.
