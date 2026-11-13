# Дадлага

ҮУПК-ийн дадлагын дэвтрийг цахимжуулсан Монгол хэлтэй портал. Vinext, React, Better Auth, Drizzle/D1, R2 болон pdf-lib ашиглана.

## Хөгжүүлэх

`npm ci`, `npm run check`, `npm run build` ажиллуулна. D1 схемийн өөрчлөлтөд `npm run db:generate` ашиглаж шинэ migration нэмнэ. Өмнө хэрэглэсэн migration-ыг өөрчлөхгүй.

Локал ажиллуулахад `.env.example`-ийг үл хянагдах `.dev.vars` файлд хуулж, `SITE_URL=http://localhost:4173` болон өөрийн нууц тохиргоог бөглөнө. Build хийсний дараа local D1-д migration бүрийг дарааллаар `node node_modules/wrangler/bin/wrangler.js d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0000_brainy_timeslip.sql` гэж хэрэглээд `npm run dev` ажиллуулна. Production болон local өгөгдлийн сан тусдаа.

Production нууц тохиргоог Sites-д хадгална. `.env.example` нь зөвхөн хувьсагчийн нэрсийг харуулна. Эх кодод нууц үг, session secret эсвэл credential хадгалахгүй. `scripts/prepare-secrets.mjs` нууц үгийг stdin-ээс авч Better Auth-ийн хэш үүсгэнэ. Анхны админы данс идемпотент seed-ээр нэг удаа үүсэх бөгөөд дахин нийтлэхэд өөрчилсөн нууц үгийг дарж бичихгүй.

## Шалгах

Build хийсний дараа `npm test` ажиллуулна. Suite нь түр Miniflare D1/R2 ашиглаж, бүртгэлийн зөвшөөрөл, серверийн эрх, хадгалалт, хяналтын урсгал, оноо, хувийн хавсралт, нууц үг шинэчлэлт, PDF үүсгэлтийг шалгана. Production өгөгдөлд хандахгүй. Туршилтын PDF `outputs/test-report.pdf` үүснэ.

## Нийтлэх

Sites-ийн project ID `.openai/hosting.json`-д хадгалагдана. Worker entry `dist/server/index.js`, статик файлууд `dist/client`, D1 migration-ууд `drizzle/` байна. Source commit-ыг холбоотой repository руу push хийсний дараа `.openai/hosting.json`, `dist/`, `drizzle/` агуулсан архивыг Sites-д хадгалж нийтэлнэ. Энэ орчинд Sites-ийн локал source/build helper суулгагдаагүй тул ижил supported artifact форматыг framework build ба `scripts/sync-source.mjs`-ээр бэлтгэдэг. Credential зөвхөн stdin болон Git процессын environment-д дамжина.

Одоогийн нийтлэлт: source credential шинэчлэх native үйлдэл `sites_git_client_upgrade_required` алдаатай буцсан. Site үүссэн, runtime тохиргоо хадгалагдсан; source push, version save, deploy болон public access хараахан хийгдээгүй. Дэмжигдэх hosted Work эсвэл шинэчлэгдсэн клиентээс одоо байгаа project ID-г ашиглан credential авч, source push → version save → public access → deploy урсгалыг үргэлжлүүлнэ. Шинэ Site үүсгэхгүй. Browser QA tool энэ орчинд байхгүй тул харагдац браузераар шалгагдаагүй.

Эх PDF, лого болон Монгол үсгийн Noto Sans фонтууд `public/assets`-д орсон. PDF-д PNG/JPEG зураг оруулж, WebP/PDF хавсралтын нэрсийг жагсаана. Бүх эх хавсралтыг эрхийн шалгалттайгаар татаж авна.
