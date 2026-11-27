# POLICY / 方針 (rs-whisper)

[日本語](#日本語) · [English](#english) · [简体中文](#简体中文) · [繁體中文](#繁體中文) · [한국어](#한국어) · [Deutsch](#deutsch) · [Français](#français) · [Español](#español) · [Português](#português) · [Русский](#русский) · [العربية](#العربية) · [فارسی](#فارسی)

> Translations are AI-made, not yet checked by native speakers. / 翻訳は AI によるもので、ネイティブ確認は未了です。

## 日本語
1. **ライセンス**: MIT。whisper.cpp(MIT)由来で、元の著作権表示(`LICENSE`)を残す。依存や再利用するコードは MIT と両立するもの(例: Apache-2.0)だけにする。
2. **正直な報告**: 「できた」と書くのは、C++ 版と同じ入力で出力を比べ、一致を確かめた後だけ。一致しない・未確認の部分は、そのまま書く。
3. **API**: 新しい REST は作らない。外から呼ぶ入口は Connect(`POST /<パッケージ>.<Service>/<Method>`、JSON)、サービス間は gRPC。JSON は RS-JSON を使い、他の JSON 部品と二重に使わない。
4. **Node.js は使わない**。
5. **秘密を書かない**: パスワード・トークン・鍵を、リポジトリ・ログ・文書に入れない。
6. **場所**: 開発・試験は手元 PC(WSL)。VPS は読み取りと、バックアップ付きの配備確認だけ。
7. **モデルの取得**: 公式の配布元からだけ取得し、ハッシュ(SHA-1)を確かめる。取得先・サイズ・ハッシュを記録する。
8. **文書**: README / CLAUDE / PORTING / POLICY は日本語・English・簡体中文・繁體中文・한국어・Deutsch・Français・Español・Português・Русский・العربية・فارسی の 12 言語で書く。AI 翻訳であることを明記し、ネイティブ確認の結果を記録する。

## English
1. **License**: MIT. It derives from whisper.cpp (MIT), so the original copyright notice (`LICENSE`) stays. Dependencies and reused code must be compatible with MIT (for example Apache-2.0).
2. **Honest reporting**: write "done" only after comparing output with the C++ version on the same input and confirming a match. Say plainly what does not match or has not been checked.
3. **API**: no new REST. Entry points called from outside use Connect (`POST /<package>.<Service>/<Method>`, JSON); service-to-service uses gRPC. JSON goes through RS-JSON, never together with another JSON library.
4. **No Node.js.**
5. **No secrets**: never put passwords, tokens or keys in the repository, logs or documents.
6. **Where we work**: development and testing on the local PC (WSL). The VPS is only for reading and for deployment checks with a backup.
7. **Model downloads**: only from the official source, with the hash (SHA-1) verified. Record the source, size and hash.
8. **Documents**: README / CLAUDE / PORTING / POLICY are written in 12 languages: Japanese, English, Simplified Chinese, Traditional Chinese, Korean, German, French, Spanish, Portuguese, Russian, Arabic and Persian. State that they are AI translations and record the result of native-speaker review.

## 简体中文
1. **许可证**:MIT。本项目源自 whisper.cpp(MIT),因此保留原版权声明(`LICENSE`)。依赖和复用的代码必须与 MIT 兼容(例如 Apache-2.0)。
2. **如实汇报**:只有在用同样的输入与 C++ 版对比输出并确认一致之后,才写“完成”。不一致或尚未验证的部分要直接写明。
3. **API**:不新增 REST。对外调用的入口使用 Connect(`POST /<包名>.<Service>/<Method>`,JSON);服务之间使用 gRPC。JSON 统一使用 RS-JSON,不与其他 JSON 库混用。
4. **不使用 Node.js。**
5. **不写入机密**:密码、令牌和密钥不得出现在仓库、日志或文档中。
6. **工作地点**:开发和测试在本地电脑(WSL)上进行。VPS 只用于读取,以及带备份的部署检查。
7. **模型下载**:只从官方来源下载,并校验哈希(SHA-1)。记录来源、大小和哈希值。
8. **文档**:README / CLAUDE / PORTING / POLICY 用 12 种语言书写:日语、英语、简体中文、繁体中文、韩语、德语、法语、西班牙语、葡萄牙语、俄语、阿拉伯语和波斯语。须注明为 AI 翻译,并记录母语者审校的结果。

## 繁體中文
1. **授權**:MIT。本專案源自 whisper.cpp(MIT),因此保留原著作權聲明(`LICENSE`)。相依套件與重用的程式碼必須與 MIT 相容(例如 Apache-2.0)。
2. **如實回報**:只有在以相同輸入與 C++ 版比對輸出並確認一致之後,才寫「完成」。不一致或尚未驗證的部分要直接寫明。
3. **API**:不新增 REST。對外呼叫的入口使用 Connect(`POST /<套件名>.<Service>/<Method>`,JSON);服務之間使用 gRPC。JSON 統一使用 RS-JSON,不與其他 JSON 函式庫混用。
4. **不使用 Node.js。**
5. **不寫入機密**:密碼、權杖與金鑰不得出現在儲存庫、日誌或文件中。
6. **工作地點**:開發與測試在本機電腦(WSL)上進行。VPS 僅用於讀取,以及附有備份的部署檢查。
7. **模型下載**:只從官方來源下載,並驗證雜湊值(SHA-1)。記錄來源、大小與雜湊值。
8. **文件**:README / CLAUDE / PORTING / POLICY 以 12 種語言撰寫:日語、英語、簡體中文、繁體中文、韓語、德語、法語、西班牙語、葡萄牙語、俄語、阿拉伯語與波斯語。須註明為 AI 翻譯,並記錄母語人士審閱的結果。

## 한국어
1. **라이선스**: MIT입니다. whisper.cpp(MIT)에서 파생되었으므로 원래의 저작권 표시(`LICENSE`)를 유지합니다. 의존성과 재사용하는 코드는 MIT와 호환되는 것(예: Apache-2.0)만 씁니다.
2. **정직한 보고**: C++ 버전과 같은 입력으로 출력을 비교해 일치를 확인한 뒤에만 "완료"라고 씁니다. 일치하지 않거나 확인하지 못한 부분은 그대로 적습니다.
3. **API**: 새 REST는 만들지 않습니다. 외부에서 호출하는 입구는 Connect(`POST /<패키지>.<Service>/<Method>`, JSON)를, 서비스 사이는 gRPC를 씁니다. JSON은 RS-JSON으로 처리하며 다른 JSON 라이브러리와 함께 쓰지 않습니다.
4. **Node.js는 쓰지 않습니다.**
5. **비밀 정보를 적지 않습니다**: 비밀번호, 토큰, 키를 저장소·로그·문서에 넣지 않습니다.
6. **작업 장소**: 개발과 시험은 로컬 PC(WSL)에서 합니다. VPS는 읽기와 백업을 갖춘 배포 확인에만 씁니다.
7. **모델 내려받기**: 공식 배포처에서만 받고 해시(SHA-1)를 확인합니다. 받은 곳, 크기, 해시를 기록합니다.
8. **문서**: README / CLAUDE / PORTING / POLICY는 일본어, 영어, 중국어 간체, 중국어 번체, 한국어, 독일어, 프랑스어, 스페인어, 포르투갈어, 러시아어, 아랍어, 페르시아어의 12개 언어로 씁니다. AI 번역임을 밝히고 원어민 검토 결과를 기록합니다.

## Deutsch
1. **Lizenz**: MIT. Das Projekt leitet sich von whisper.cpp (MIT) ab; daher bleibt der ursprüngliche Urheberrechtshinweis (`LICENSE`) erhalten. Abhängigkeiten und wiederverwendeter Code müssen mit MIT vereinbar sein (zum Beispiel Apache-2.0).
2. **Ehrliche Berichte**: „Fertig“ schreiben wir erst, nachdem die Ausgabe bei gleicher Eingabe mit der C++-Version verglichen und als übereinstimmend bestätigt wurde. Was nicht übereinstimmt oder ungeprüft ist, wird offen benannt.
3. **API**: keine neuen REST-Schnittstellen. Von außen aufgerufene Einstiegspunkte nutzen Connect (`POST /<Paket>.<Service>/<Methode>`, JSON), zwischen Diensten wird gRPC verwendet. JSON läuft über RS-JSON, nie zusammen mit einer anderen JSON-Bibliothek.
4. **Kein Node.js.**
5. **Keine Geheimnisse**: Passwörter, Token und Schlüssel gehören weder ins Repository noch in Logs oder Dokumente.
6. **Arbeitsort**: Entwicklung und Tests auf dem lokalen PC (WSL). Der VPS dient nur zum Lesen und für Deployment-Prüfungen mit Sicherung.
7. **Modell-Downloads**: nur von der offiziellen Quelle und mit geprüftem Hash (SHA-1). Quelle, Größe und Hash werden festgehalten.
8. **Dokumente**: README / CLAUDE / PORTING / POLICY entstehen in 12 Sprachen: Japanisch, Englisch, vereinfachtes Chinesisch, traditionelles Chinesisch, Koreanisch, Deutsch, Französisch, Spanisch, Portugiesisch, Russisch, Arabisch und Persisch. Es wird vermerkt, dass es sich um KI-Übersetzungen handelt, und das Ergebnis der Prüfung durch Muttersprachler wird festgehalten.

## Français
1. **Licence** : MIT. Le projet dérive de whisper.cpp (MIT) ; la mention de droits d'auteur d'origine (`LICENSE`) est donc conservée. Les dépendances et le code réutilisé doivent être compatibles avec la licence MIT (par exemple Apache-2.0).
2. **Rapports honnêtes** : on n'écrit « terminé » qu'après avoir comparé la sortie avec celle de la version C++ sur la même entrée et confirmé la concordance. Ce qui ne concorde pas ou n'a pas été vérifié est dit clairement.
3. **API** : pas de nouvelle API REST. Les points d'entrée appelés de l'extérieur utilisent Connect (`POST /<paquet>.<Service>/<Méthode>`, JSON) ; entre services, on utilise gRPC. Le JSON passe par RS-JSON, jamais en parallèle d'une autre bibliothèque JSON.
4. **Pas de Node.js.**
5. **Pas de secrets** : aucun mot de passe, jeton ou clé dans le dépôt, les journaux ou les documents.
6. **Lieu de travail** : développement et tests sur le PC local (WSL). Le VPS ne sert qu'à la lecture et aux vérifications de déploiement avec sauvegarde.
7. **Téléchargement de modèles** : uniquement depuis la source officielle, avec vérification de l'empreinte (SHA-1). La source, la taille et l'empreinte sont consignées.
8. **Documents** : README / CLAUDE / PORTING / POLICY sont rédigés en 12 langues : japonais, anglais, chinois simplifié, chinois traditionnel, coréen, allemand, français, espagnol, portugais, russe, arabe et persan. On précise qu'il s'agit de traductions par IA et on consigne le résultat de la relecture par des locuteurs natifs.

## Español
1. **Licencia**: MIT. El proyecto deriva de whisper.cpp (MIT), por lo que se conserva el aviso de derechos de autor original (`LICENSE`). Las dependencias y el código reutilizado deben ser compatibles con MIT (por ejemplo, Apache-2.0).
2. **Informes honestos**: solo se escribe «terminado» después de comparar la salida con la de la versión en C++ con la misma entrada y confirmar que coincide. Lo que no coincide o no se ha comprobado se dice con claridad.
3. **API**: no se crean nuevas API REST. Los puntos de entrada que se llaman desde fuera usan Connect (`POST /<paquete>.<Service>/<Método>`, JSON); entre servicios se usa gRPC. El JSON pasa por RS-JSON, nunca junto con otra biblioteca JSON.
4. **Sin Node.js.**
5. **Sin secretos**: no se incluyen contraseñas, tokens ni claves en el repositorio, los registros ni los documentos.
6. **Dónde se trabaja**: el desarrollo y las pruebas se hacen en el PC local (WSL). El VPS solo se usa para lectura y para comprobar despliegues con copia de seguridad.
7. **Descarga de modelos**: solo desde la fuente oficial y con el hash (SHA-1) verificado. Se anotan la fuente, el tamaño y el hash.
8. **Documentos**: README / CLAUDE / PORTING / POLICY se escriben en 12 idiomas: japonés, inglés, chino simplificado, chino tradicional, coreano, alemán, francés, español, portugués, ruso, árabe y persa. Se indica que son traducciones hechas con IA y se registra el resultado de la revisión por hablantes nativos.

## Português
1. **Licença**: MIT. O projeto deriva do whisper.cpp (MIT), por isso o aviso de direitos autorais original (`LICENSE`) é mantido. Dependências e código reutilizado devem ser compatíveis com a MIT (por exemplo, Apache-2.0).
2. **Relatos honestos**: só escrevemos «concluído» depois de comparar a saída com a da versão em C++ na mesma entrada e confirmar que coincide. O que não coincide ou não foi verificado é dito com clareza.
3. **API**: nada de novas APIs REST. Os pontos de entrada chamados de fora usam Connect (`POST /<pacote>.<Service>/<Método>`, JSON); entre serviços usa-se gRPC. O JSON passa pelo RS-JSON, nunca junto com outra biblioteca JSON.
4. **Sem Node.js.**
5. **Sem segredos**: nenhuma senha, token ou chave no repositório, nos logs ou nos documentos.
6. **Onde trabalhamos**: desenvolvimento e testes no PC local (WSL). O VPS serve apenas para leitura e para verificações de implantação com backup.
7. **Download de modelos**: somente da fonte oficial, com o hash (SHA-1) verificado. Registram-se a fonte, o tamanho e o hash.
8. **Documentos**: README / CLAUDE / PORTING / POLICY são escritos em 12 idiomas: japonês, inglês, chinês simplificado, chinês tradicional, coreano, alemão, francês, espanhol, português, russo, árabe e persa. Indica-se que são traduções feitas por IA e registra-se o resultado da revisão por falantes nativos.

## Русский
1. **Лицензия**: MIT. Проект произведён от whisper.cpp (MIT), поэтому исходное уведомление об авторских правах (`LICENSE`) сохраняется. Зависимости и повторно используемый код должны быть совместимы с MIT (например, Apache-2.0).
2. **Честные отчёты**: «готово» пишем только после того, как результат сравнён с версией на C++ на одних и тех же входных данных и совпадение подтверждено. О том, что не совпадает или не проверено, говорим прямо.
3. **API**: новых REST-интерфейсов не создаём. Точки входа, которые вызываются снаружи, используют Connect (`POST /<пакет>.<Service>/<Method>`, JSON); между сервисами применяется gRPC. JSON обрабатывается через RS-JSON и никогда не совмещается с другой библиотекой JSON.
4. **Node.js не используем.**
5. **Секреты не записываем**: пароли, токены и ключи не попадают ни в репозиторий, ни в журналы, ни в документы.
6. **Где работаем**: разработка и тесты — на локальном ПК (WSL). VPS нужен только для чтения и проверки развёртывания с резервной копией.
7. **Загрузка моделей**: только из официального источника, с проверкой хеша (SHA-1). Источник, размер и хеш записываются.
8. **Документы**: README / CLAUDE / PORTING / POLICY пишутся на 12 языках: японском, английском, упрощённом и традиционном китайском, корейском, немецком, французском, испанском, португальском, русском, арабском и персидском. Указываем, что это перевод, сделанный ИИ, и записываем результат проверки носителями языка.

## العربية
<div dir="rtl">

1. **الترخيص**: MIT. المشروع مشتق من whisper.cpp (بترخيص MIT)، لذا يبقى إشعار حقوق النشر الأصلي (`LICENSE`) كما هو. ويجب أن تكون الاعتماديات والشيفرة المُعاد استخدامها متوافقة مع MIT (مثل Apache-2.0).
2. **الإبلاغ بصدق**: لا نكتب «اكتمل» إلا بعد مقارنة المخرجات بإصدار C++ على المدخلات نفسها والتأكد من التطابق. وما لا يتطابق أو لم يُتحقَّق منه نذكره صراحةً.
3. **واجهات البرمجة**: لا نُنشئ واجهات REST جديدة. تستخدم نقاط الدخول التي تُستدعى من الخارج بروتوكول Connect (‏`POST /<الحزمة>.<Service>/<Method>`، بصيغة JSON)، وتستخدم الاتصالات بين الخدمات بروتوكول gRPC. وتُعالَج صيغة JSON عبر RS-JSON ولا تُستخدم معها مكتبة JSON أخرى.
4. **لا نستخدم Node.js.**
5. **لا أسرار في الملفات**: لا تُكتب كلمات المرور ولا الرموز المميّزة ولا المفاتيح في المستودع أو السجلات أو الوثائق.
6. **مكان العمل**: التطوير والاختبار على الحاسوب المحلي (WSL). أما الخادم VPS فيُستخدم للقراءة وللتحقق من النشر مع نسخة احتياطية فقط.
7. **تنزيل النماذج**: من المصدر الرسمي فقط، مع التحقق من قيمة التجزئة (SHA-1)، وتُسجَّل المصدر والحجم والتجزئة.
8. **الوثائق**: تُكتب README وCLAUDE وPORTING وPOLICY بـ 12 لغة: اليابانية والإنجليزية والصينية المبسّطة والصينية التقليدية والكورية والألمانية والفرنسية والإسبانية والبرتغالية والروسية والعربية والفارسية. ويُذكر أنها ترجمات بالذكاء الاصطناعي، وتُسجَّل نتيجة مراجعة الناطقين الأصليين.

</div>

## فارسی
<div dir="rtl">

۱. **مجوز**: MIT. این پروژه از whisper.cpp (با مجوز MIT) مشتق شده است، پس اعلان حق نشر اصلی (`LICENSE`) حفظ می‌شود. وابستگی‌ها و کدهای بازاستفاده‌شده باید با MIT سازگار باشند (مثلاً Apache-2.0).
۲. **گزارش صادقانه**: فقط پس از مقایسهٔ خروجی با نسخهٔ C++ روی ورودی یکسان و تأیید تطابق، می‌نویسیم «انجام شد». آنچه مطابقت ندارد یا بررسی نشده است را صریحاً می‌گوییم.
۳. **API**: هیچ REST تازه‌ای ساخته نمی‌شود. نقاط ورودی‌ای که از بیرون فراخوانی می‌شوند از Connect (‏`POST /<بسته>.<Service>/<Method>`، با JSON) و ارتباط میان سرویس‌ها از gRPC استفاده می‌کند. JSON از طریق RS-JSON پردازش می‌شود و هرگز همراه با کتابخانهٔ JSON دیگری به کار نمی‌رود.
۴. **از Node.js استفاده نمی‌کنیم.**
۵. **رازی نمی‌نویسیم**: گذرواژه، توکن و کلید در مخزن، گزارش‌ها و اسناد قرار نمی‌گیرد.
۶. **محل کار**: توسعه و آزمایش روی رایانهٔ محلی (WSL) انجام می‌شود. VPS فقط برای خواندن و بررسی استقرار همراه با پشتیبان به کار می‌رود.
۷. **دریافت مدل‌ها**: فقط از منبع رسمی و با راستی‌آزمایی هش (SHA-1). منبع، اندازه و هش ثبت می‌شود.
۸. **اسناد**: README و CLAUDE و PORTING و POLICY به ۱۲ زبان نوشته می‌شوند: ژاپنی، انگلیسی، چینی ساده، چینی سنتی، کره‌ای، آلمانی، فرانسوی، اسپانیایی، پرتغالی، روسی، عربی و فارسی. ذکر می‌شود که ترجمه‌ها با هوش مصنوعی انجام شده‌اند و نتیجهٔ بازبینی گویشورانِ بومی ثبت می‌شود.

</div>

## 2026-11-23 rs-whisper-la / word times
日本語: `rs-whisper-ts`(語時刻の確認)と `rs-whisper-la`(LocalAgreementのストリーミング試作、語時刻で切り捨て)を追加。詳細は PORTING.md 末尾。
English: Added `rs-whisper-ts` (word-time check) and `rs-whisper-la` (LocalAgreement streaming prototype trimming by word times). See the end of PORTING.md.

## Versionless API の方針 / Versionless API policy (2026-11-25)
**日本語**: このリポジトリのAPIは **Versionless**(URL・パッケージ名に `v1` 等を付けず、互換性を壊さず足すだけで進化させる)。欄は足すだけ・足す欄は省略可能・知らない欄は無視・名前変更は新名を足して旧を非推奨に(期限は既定で6か月以上、使用0を確認してから削除)・enumは未知の値を許容・安全上の緊急修正は例外。既存の `.v1` は呼び出し側を壊さないよう当面残し、版なしの別名を追加して段階的に畳む。詳細・出典は [`runo/API-POLICY.md`](https://github.com/aon-co-jp/runo/blob/main/API-POLICY.md) 末尾。適用はこれから順に行う。
**English**: This repository's APIs are **Versionless**: no `v1` in URLs or package names; evolve by adding only, without breaking compatibility. Only add fields, new fields are optional, ignore unknown fields, rename by adding the new name and deprecating the old (default deadline 6 months or more, remove only after confirming zero use), tolerate unknown enum values, emergency security fixes are the exception. Existing `.v1` names stay for now so callers do not break; a versionless alias is added and `.v1` is retired in stages. Details and sources: the end of [`runo/API-POLICY.md`](https://github.com/aon-co-jp/runo/blob/main/API-POLICY.md). Rollout is step by step.

## API 方針: REST 全廃・gRPC / Connect へ
**日本語**: このリポジトリでは **REST API を新規に作らない**。ブラウザ・`curl` から呼ぶ API は **Connect の単純呼び出し**(`POST /<パッケージ>.<Service>/<Method>`、`content-type: application/json`)、サービス間は **gRPC / gRPC-web**。GraphQL は読み取り専用。既存の REST は触るときに移し、移したら旧経路は消す。JSON は RS-JSON(`from_str_secure` / `parse_secure`)で読む。例外は相手の規格が形を決めるもの(ACME、OAuth/OIDC のコールバック、第三者の Webhook、ヘルスチェック、静的ファイル、OTLP)のみ。迷ったら Connect。正本は [`runo/API-POLICY.md`](https://github.com/aon-co-jp/runo/blob/main/API-POLICY.md)。
**English**: This repository **does not add new REST APIs**. APIs called from browsers or `curl` use **Connect unary calls** (`POST /<package>.<Service>/<Method>`, `content-type: application/json`); service-to-service calls use **gRPC / gRPC-web**. GraphQL is read-only. Existing REST is migrated when touched and the old route is removed. Read JSON with RS-JSON (`from_str_secure` / `parse_secure`). Exceptions are only shapes dictated by someone else's standard (ACME, OAuth/OIDC callbacks, third-party webhooks, health checks, static files, OTLP). When in doubt, use Connect. Source of truth: the link above.

## 完成まで作り切る方針 / Finish everything policy (2026-11-27)
**日本語**: 全リポジトリで、**未確認は自動で確認し、未実施は実施し、未了は完成させる**(確認を求めず進める)。使用頻度が低いと判断される機能や、**1から作り直す大規模開発になる内容でも、やめずに最後まで開発・TEST・DEBUGする**(別の方式で済ませない、後回しにしない)。進め方: 調査(英語・日本語の検索とGitHub) → 設計 → 実装 → 実機/実データで確認 → 失敗は原因を直して再試験 → README・CLAUDE・PORTING・POLICY を日英で更新 → push。「できた」と書くのは実測した範囲だけ、確認できなかった所は「未確認」と正直に書く。例外(確認を取る): 元に戻せない削除、秘密の公開、所有者の認証・決済・実機操作が要るもの、利用者のシステム設定の変更。
**English**: In every repository, **verify what is unverified, do what is not yet done, and finish what is unfinished** automatically, without asking. Even features that look rarely used, or work that means **a large rewrite from scratch, are developed, tested and debugged to the end** (no substituting another approach, no postponing). Method: research (English and Japanese search plus GitHub) -> design -> implement -> verify on real devices or data -> fix causes and retest on failure -> update README, CLAUDE, PORTING and POLICY in Japanese and English -> push. Write "done" only for what was measured, and say "unverified" honestly for the rest. Exceptions that still need confirmation: irreversible deletions, publishing secrets, anything needing the owner's authentication, payment or real-device actions, and changes to the user's system settings.