# CLAUDE.md — rs-whisper

whisper.cpp(MIT)を、段階ごとに C++ の出力と突き合わせながら Rust へ移すリポジトリです。ライセンスは元の MIT を引き継ぎ、`LICENSE` に原著作者の表示を残します。方針は [`POLICY.md`](POLICY.md)、進み具合は [`PORTING.md`](PORTING.md)、使い方は [`README.md`](README.md)。

## 日本語
**AI(Claude)が作業するときの決まり**
- 「できた」と書くのは、C++ 版(`whisper-cli`)と同じ入力で出力を比べ、一致を確かめてから。一致しなかった・測っていないことは、そのまま書く。
- 開発・テストは手元PC(WSL)。VPS は読み取りだけ。新しい REST は作らない(Connect/gRPC のみ)。JSON は RS-JSON だけ。Node.js は使わない。秘密は書かない。
- 基準の測り方: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <モデル.bin> <音声.wav>`(最後に `TRANSCRIPT IDENTICAL` が出れば一致)。メルは `tests/ref/run.sh`・`mel_real.sh`、モデル読み込みは `model_run.sh`。
- 落とし穴: Git Bash には python3 が無く `/mnt` を書き換える → WSL の命令は PowerShell から。複数行の処理は `.sh` に書く。モデルは `F:\_work\models\ggml-tiny.bin`(SHA-1 bd577a11…)。
- 確認済み: メル 328000/328000 が完全一致、モデル読み込み(167テンソル)が独立実装と一致、tiny + jfk.wav の文字起こしが whisper.cpp と一致(エンコード約3〜4秒)。
- 未対応(正直に): 量子化重み、ビームサーチ、時刻付き出力、英語以外、CLI/サーバー、GPU、エンコーダ途中値の数値比較、GELU は tanh 近似(ggml の表ではない)、tiny 以外の実重み。
- 文書は12言語(日・英・簡・繁・韓・独・仏・西・葡・露・アラビア語・ペルシャ語)。AI 翻訳でネイティブ検証前、と必ず明記する。

## English
**Rules when an AI (Claude) works here**
- Write "done" only after comparing output with the C++ build (`whisper-cli`) on the same input. State plainly anything that differed or was not measured.
- Develop and test on the local PC (WSL); the VPS is read-only. No new REST (Connect/gRPC only); JSON via RS-JSON only; no Node.js; never write secrets.
- How to measure: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <model.bin> <audio.wav>` (`TRANSCRIPT IDENTICAL` at the end means a match). Mel: `tests/ref/run.sh`, `mel_real.sh`; model loading: `model_run.sh`.
- Pitfalls: Git Bash has no python3 and rewrites `/mnt` paths, so run WSL commands from PowerShell and keep multi-line logic in `.sh` files. Model: `F:\_work\models\ggml-tiny.bin` (SHA-1 bd577a11…).
- Verified: mel 328000/328000 bit-identical; model loader (167 tensors) identical to an independent reader; tiny + jfk.wav transcript identical to whisper.cpp (encode about 3–4 s).
- Not done (honestly): quantized weights, beam search, timestamps, non-English, CLI/server, GPU, numeric comparison of encoder intermediates, GELU is the tanh approximation (not ggml's table), real weights other than tiny.
- Docs are in 12 languages and must say they are AI translations not yet checked by native speakers.

## 简体中文
**AI(Claude)在此工作时的规则**
- 只有在用同一输入与 C++ 版(`whisper-cli`)比较输出并确认一致后,才能写“完成”。不一致或未测量的内容要如实写明。
- 开发和测试在本地电脑(WSL)上进行,VPS 只读。不新增 REST(仅 Connect/gRPC);JSON 只用 RS-JSON;不用 Node.js;不写入任何机密。
- 测量方法:`wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <模型.bin> <音频.wav>`(末尾出现 `TRANSCRIPT IDENTICAL` 即一致)。梅尔:`tests/ref/run.sh`、`mel_real.sh`;模型读取:`model_run.sh`。
- 注意:Git Bash 没有 python3 且会改写 `/mnt` 路径,所以 WSL 命令要从 PowerShell 运行,多行逻辑写进 `.sh`。模型为 `F:\_work\models\ggml-tiny.bin`(SHA-1 bd577a11…)。
- 已确认:梅尔 328000/328000 完全一致;模型读取(167 个张量)与独立实现一致;tiny + jfk.wav 的转写与 whisper.cpp 一致(编码约 3–4 秒)。
- 未完成(如实):量化权重、束搜索、时间戳、非英语、CLI/服务器、GPU、编码器中间值的数值比较、GELU 为 tanh 近似(不是 ggml 的表)、tiny 以外的真实权重。
- 文档使用 12 种语言,并且必须注明是 AI 翻译、尚未经母语者审校。

## 繁體中文
**AI(Claude)在此工作時的規則**
- 只有在以相同輸入與 C++ 版(`whisper-cli`)比對輸出並確認一致後,才能寫「完成」。不一致或未量測的內容要如實寫明。
- 開發與測試在本機電腦(WSL)進行,VPS 唯讀。不新增 REST(僅 Connect/gRPC);JSON 只用 RS-JSON;不用 Node.js;不寫入任何機密。
- 量測方式:`wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <模型.bin> <音訊.wav>`(結尾出現 `TRANSCRIPT IDENTICAL` 即一致)。梅爾:`tests/ref/run.sh`、`mel_real.sh`;模型讀取:`model_run.sh`。
- 注意:Git Bash 沒有 python3 且會改寫 `/mnt` 路徑,所以 WSL 指令要從 PowerShell 執行,多行邏輯寫進 `.sh`。模型為 `F:\_work\models\ggml-tiny.bin`(SHA-1 bd577a11…)。
- 已確認:梅爾 328000/328000 完全一致;模型讀取(167 個張量)與獨立實作一致;tiny + jfk.wav 的轉寫與 whisper.cpp 一致(編碼約 3–4 秒)。
- 未完成(如實):量化權重、束搜尋、時間戳、非英語、CLI/伺服器、GPU、編碼器中間值的數值比對、GELU 為 tanh 近似(不是 ggml 的表)、tiny 以外的真實權重。
- 文件使用 12 種語言,並且必須註明是 AI 翻譯、尚未經母語人士審閱。

## 한국어
**AI(Claude)가 여기서 작업할 때의 규칙**
- 같은 입력으로 C++ 버전(`whisper-cli`)과 출력을 비교해 일치를 확인한 뒤에만 "완료"라고 씁니다. 일치하지 않았거나 측정하지 않은 것은 있는 그대로 적습니다.
- 개발과 테스트는 로컬 PC(WSL)에서, VPS는 읽기 전용입니다. 새 REST는 만들지 않고(Connect/gRPC만), JSON은 RS-JSON만, Node.js는 쓰지 않으며, 비밀은 적지 않습니다.
- 측정 방법: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <모델.bin> <오디오.wav>`(끝에 `TRANSCRIPT IDENTICAL`이 나오면 일치). 멜: `tests/ref/run.sh`, `mel_real.sh`; 모델 읽기: `model_run.sh`.
- 함정: Git Bash에는 python3가 없고 `/mnt` 경로를 바꿔 버리므로, WSL 명령은 PowerShell에서 실행하고 여러 줄 처리는 `.sh`에 둡니다. 모델: `F:\_work\models\ggml-tiny.bin`(SHA-1 bd577a11…).
- 확인됨: 멜 328000/328000 완전 일치, 모델 읽기(텐서 167개)가 독립 구현과 일치, tiny + jfk.wav 전사가 whisper.cpp와 일치(인코딩 약 3~4초).
- 미완료(솔직히): 양자화 가중치, 빔 서치, 타임스탬프, 영어 이외, CLI/서버, GPU, 인코더 중간값의 수치 비교, GELU는 tanh 근사(ggml의 표가 아님), tiny 이외의 실제 가중치.
- 문서는 12개 언어로 쓰고, AI 번역이며 원어민 검토 전이라는 점을 반드시 밝힙니다.

## Deutsch
**Regeln für die Arbeit einer KI (Claude) hier**
- „Fertig“ erst schreiben, wenn die Ausgabe mit der C++-Version (`whisper-cli`) bei gleicher Eingabe verglichen und als gleich bestätigt wurde. Abweichungen und nicht Gemessenes offen nennen.
- Entwickeln und Testen auf dem lokalen PC (WSL); der VPS bleibt nur lesbar. Keine neue REST-API (nur Connect/gRPC), JSON nur über RS-JSON, kein Node.js, keine Geheimnisse notieren.
- Messung: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <Modell.bin> <Audio.wav>` (steht am Ende `TRANSCRIPT IDENTICAL`, stimmt es überein). Mel: `tests/ref/run.sh`, `mel_real.sh`; Modell laden: `model_run.sh`.
- Stolperfallen: Git Bash hat kein python3 und schreibt `/mnt`-Pfade um; WSL-Befehle daher aus PowerShell starten und mehrzeilige Logik in `.sh`-Dateien legen. Modell: `F:\_work\models\ggml-tiny.bin` (SHA-1 bd577a11…).
- Geprüft: Mel 328000/328000 bitgleich; Modell-Lader (167 Tensoren) stimmt mit einem unabhängigen Leser überein; Transkript von tiny + jfk.wav identisch mit whisper.cpp (Encoder ca. 3–4 s).
- Nicht erledigt (ehrlich): quantisierte Gewichte, Beam Search, Zeitstempel, andere Sprachen als Englisch, CLI/Server, GPU, numerischer Vergleich der Encoder-Zwischenwerte, GELU ist die tanh-Näherung (nicht die ggml-Tabelle), echte Gewichte außer tiny.
- Die Dokumente gibt es in 12 Sprachen und sie müssen sagen, dass es KI-Übersetzungen ohne Prüfung durch Muttersprachler sind.

## Français
**Règles quand une IA (Claude) travaille ici**
- N'écrire « terminé » qu'après avoir comparé la sortie avec la version C++ (`whisper-cli`) sur la même entrée et confirmé qu'elle est identique. Dire franchement ce qui diffère ou n'a pas été mesuré.
- Développer et tester sur le PC local (WSL) ; le VPS reste en lecture seule. Pas de nouvelle API REST (Connect/gRPC uniquement), JSON via RS-JSON uniquement, pas de Node.js, aucun secret écrit.
- Mesure : `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <modèle.bin> <audio.wav>` (`TRANSCRIPT IDENTICAL` en fin de sortie = identique). Mel : `tests/ref/run.sh`, `mel_real.sh` ; chargement du modèle : `model_run.sh`.
- Pièges : Git Bash n'a pas python3 et réécrit les chemins `/mnt` ; lancer les commandes WSL depuis PowerShell et garder la logique multiligne dans des `.sh`. Modèle : `F:\_work\models\ggml-tiny.bin` (SHA-1 bd577a11…).
- Vérifié : mel 328000/328000 identique bit à bit ; chargeur de modèle (167 tenseurs) identique à un lecteur indépendant ; transcription de tiny + jfk.wav identique à whisper.cpp (encodeur environ 3 à 4 s).
- Non fait (franchement) : poids quantifiés, recherche par faisceau, horodatage, autres langues que l'anglais, CLI/serveur, GPU, comparaison numérique des valeurs intermédiaires de l'encodeur, GELU en approximation tanh (pas la table de ggml), poids réels autres que tiny.
- Les documents existent en 12 langues et doivent préciser qu'il s'agit de traductions par IA, non encore relues par des locuteurs natifs.

## Español
**Reglas cuando una IA (Claude) trabaja aquí**
- Escribir «terminado» solo después de comparar la salida con la versión C++ (`whisper-cli`) con la misma entrada y confirmar que coincide. Decir con franqueza lo que difiere o no se midió.
- Desarrollar y probar en el PC local (WSL); el VPS es solo de lectura. Sin nuevas API REST (solo Connect/gRPC), JSON solo con RS-JSON, sin Node.js, sin escribir secretos.
- Cómo medir: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <modelo.bin> <audio.wav>` (si al final aparece `TRANSCRIPT IDENTICAL`, coincide). Mel: `tests/ref/run.sh`, `mel_real.sh`; carga del modelo: `model_run.sh`.
- Trampas: Git Bash no tiene python3 y reescribe las rutas `/mnt`; ejecuta los comandos de WSL desde PowerShell y deja la lógica de varias líneas en archivos `.sh`. Modelo: `F:\_work\models\ggml-tiny.bin` (SHA-1 bd577a11…).
- Comprobado: mel 328000/328000 idéntico bit a bit; el cargador del modelo (167 tensores) coincide con un lector independiente; la transcripción de tiny + jfk.wav es idéntica a la de whisper.cpp (codificador unos 3–4 s).
- Pendiente (con franqueza): pesos cuantizados, búsqueda por haz, marcas de tiempo, otros idiomas distintos del inglés, CLI/servidor, GPU, comparación numérica de valores intermedios del codificador, GELU es la aproximación tanh (no la tabla de ggml), pesos reales distintos de tiny.
- Los documentos están en 12 idiomas y deben indicar que son traducciones de IA aún no revisadas por hablantes nativos.

## Português
**Regras quando uma IA (Claude) trabalha aqui**
- Só escrever «concluído» depois de comparar a saída com a versão C++ (`whisper-cli`) com a mesma entrada e confirmar que é igual. Dizer com franqueza o que diferiu ou não foi medido.
- Desenvolver e testar no PC local (WSL); o VPS é somente leitura. Sem novas APIs REST (apenas Connect/gRPC), JSON só com RS-JSON, sem Node.js, sem escrever segredos.
- Como medir: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <modelo.bin> <áudio.wav>` (`TRANSCRIPT IDENTICAL` no final significa que é igual). Mel: `tests/ref/run.sh`, `mel_real.sh`; carregamento do modelo: `model_run.sh`.
- Armadilhas: o Git Bash não tem python3 e reescreve caminhos `/mnt`; execute os comandos do WSL pelo PowerShell e deixe a lógica de várias linhas em arquivos `.sh`. Modelo: `F:\_work\models\ggml-tiny.bin` (SHA-1 bd577a11…).
- Verificado: mel 328000/328000 idêntico bit a bit; o carregador do modelo (167 tensores) coincide com um leitor independente; a transcrição de tiny + jfk.wav é idêntica à do whisper.cpp (codificador cerca de 3–4 s).
- Não feito (com franqueza): pesos quantizados, busca em feixe, marcas de tempo, outros idiomas além do inglês, CLI/servidor, GPU, comparação numérica dos valores intermediários do codificador, GELU é a aproximação tanh (não a tabela do ggml), pesos reais além do tiny.
- Os documentos estão em 12 idiomas e devem informar que são traduções de IA ainda não revisadas por falantes nativos.

## Русский
**Правила для ИИ (Claude), работающего здесь**
- Писать «готово» только после сравнения вывода с C++-версией (`whisper-cli`) на том же входе и подтверждения совпадения. Всё, что расходится или не измерено, указывать прямо.
- Разработка и тесты — на локальном ПК (WSL); VPS только для чтения. Новых REST-API не создавать (только Connect/gRPC), JSON только через RS-JSON, Node.js не использовать, секреты не записывать.
- Как измерять: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <модель.bin> <аудио.wav>` (строка `TRANSCRIPT IDENTICAL` в конце — совпадение). Мел: `tests/ref/run.sh`, `mel_real.sh`; загрузка модели: `model_run.sh`.
- Подводные камни: в Git Bash нет python3, и он переписывает пути `/mnt`; команды WSL запускайте из PowerShell, а многострочную логику храните в `.sh`. Модель: `F:\_work\models\ggml-tiny.bin` (SHA-1 bd577a11…).
- Проверено: мел 328000/328000 совпадает побитно; загрузчик модели (167 тензоров) совпадает с независимым читателем; транскрипция tiny + jfk.wav идентична whisper.cpp (энкодер около 3–4 с).
- Не сделано (честно): квантованные веса, лучевой поиск, временные метки, языки кроме английского, CLI/сервер, GPU, численное сравнение промежуточных значений энкодера, GELU — приближение tanh (не таблица ggml), реальные веса, кроме tiny.
- Документы ведутся на 12 языках и обязаны сообщать, что это переводы ИИ, ещё не проверенные носителями языка.

## العربية
<div dir="rtl">

**قواعد عمل الذكاء الاصطناعي (Claude) هنا**
- لا تكتب «تمّ» إلا بعد مقارنة المخرجات بنسخة C++ (`whisper-cli`) على المدخل نفسه والتأكد من التطابق. وما اختلف أو لم يُقَس يُذكر بصراحة.
- التطوير والاختبار على الحاسوب المحلي (WSL)، والخادم الافتراضي (VPS) للقراءة فقط. لا واجهات REST جديدة (Connect/gRPC فقط)، وJSON عبر RS-JSON فقط، ولا Node.js، ولا تُكتب أي أسرار.
- طريقة القياس: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <النموذج.bin> <الصوت.wav>` (ظهور `TRANSCRIPT IDENTICAL` في النهاية يعني التطابق). مقياس ميل: `tests/ref/run.sh` و`mel_real.sh`؛ تحميل النموذج: `model_run.sh`.
- مزالق: ليس في Git Bash برنامج python3 وهو يعيد كتابة مسارات `/mnt`، لذا شغّل أوامر WSL من PowerShell وضع المنطق متعدد الأسطر في ملفات `.sh`. النموذج: `F:\_work\models\ggml-tiny.bin` (SHA-1 ‏bd577a11…).
- تم التحقق: مقياس ميل 328000/328000 متطابق بتًّا ببت؛ محمّل النموذج (167 موتّرًا) يطابق قارئًا مستقلًا؛ نص tiny مع jfk.wav مطابق لـ whisper.cpp (المشفّر نحو 3–4 ثوانٍ).
- لم يُنجز (بصراحة): الأوزان المكمّمة، والبحث الشعاعي، والطوابع الزمنية، واللغات غير الإنجليزية، وCLI/الخادم، وGPU، والمقارنة العددية للقيم الوسيطة في المشفّر؛ وGELU تقريب tanh (وليس جدول ggml)؛ والأوزان الحقيقية غير tiny.
- الوثائق بـ12 لغة ويجب أن تذكر أنها ترجمات بالذكاء الاصطناعي لم يراجعها ناطقون أصليون بعد.

</div>

## فارسی
<div dir="rtl">

**قواعد کار هوش مصنوعی (Claude) در اینجا**
- فقط پس از مقایسهٔ خروجی با نسخهٔ C++‎ (`whisper-cli`) روی ورودی یکسان و تأیید یکسان‌بودن بنویسید «انجام شد». هر چه فرق داشت یا اندازه‌گیری نشد را صادقانه بنویسید.
- توسعه و آزمون روی رایانهٔ محلی (WSL) است و VPS فقط خواندنی است. REST تازه نسازید (فقط Connect/gRPC)، JSON فقط با RS-JSON، بدون Node.js، و هیچ رمز و رازی ننویسید.
- روش اندازه‌گیری: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/transcribe_run.sh <مدل.bin> <صدا.wav>` (اگر در پایان `TRANSCRIPT IDENTICAL` آمد، یکسان است). مل: `tests/ref/run.sh` و `mel_real.sh`؛ بارگذاری مدل: `model_run.sh`.
- نکته‌های دردسرساز: Git Bash برنامهٔ python3 ندارد و مسیرهای `/mnt` را بازنویسی می‌کند؛ پس دستورهای WSL را از PowerShell اجرا کنید و منطق چندخطی را در فایل‌های `.sh` بگذارید. مدل: `F:\_work\models\ggml-tiny.bin` (‏SHA-1 ‏bd577a11…).
- بررسی‌شده: مل ۳۲۸۰۰۰ از ۳۲۸۰۰۰ بیت‌به‌بیت یکسان؛ بارگذار مدل (۱۶۷ تانسور) با خوانندهٔ مستقل یکسان؛ رونویسی tiny با jfk.wav با whisper.cpp یکسان (رمزگذار حدود ۳ تا ۴ ثانیه).
- انجام‌نشده (صادقانه): وزن‌های کوانتیزه، جستجوی پرتویی، برچسب زمانی، زبان‌هایی جز انگلیسی، CLI/سرور، GPU، مقایسهٔ عددی مقدارهای میانی رمزگذار؛ GELU تقریب tanh است (نه جدول ggml)؛ وزن‌های واقعی جز tiny.
- مستندات به ۱۲ زبان است و باید بگوید ترجمهٔ هوش مصنوعی است و هنوز گویشور بومی بازبینی نکرده است.

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