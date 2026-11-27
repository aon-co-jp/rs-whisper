# rs-whisper

Languages / 言語 / 语言 / 語言 / 언어 / Sprachen / Langues / Idiomas / Idiomas / Языки / اللغات / زبان‌ها:
[日本語](#日本語) · [English](#english) · [简体中文](#简体中文) · [繁體中文](#繁體中文) · [한국어](#한국어) · [Deutsch](#deutsch) · [Français](#français) · [Español](#español) · [Português](#português) · [Русский](#русский) · [العربية](#العربية) · [فارسی](#فارسی)

> Translations are AI-made and have not yet been checked by native speakers. / 翻訳は AI によるもので、ネイティブによる確認は未了です。

## 日本語
[whisper.cpp](https://github.com/ggml-org/whisper.cpp)(MIT)を、段階的に Rust へ移植するリポジトリです。音声認識(文字起こし)の道具です。

**いまできること(2026-11-21、手元PCのCPUで確認)**
- 段階1: 音声を「メルスペクトログラム」に変える部分。C++ 版と **32.8万個の値がすべてビット単位で一致**(合成フィルタと、モデルファイルから読んだ本物のフィルタの両方)。
- 段階2: ggml 形式のモデルファイルの読み込み。本物の `ggml-tiny`(167個のテンソル)で、別に書いた Python の読み取りと内容が完全に一致。
- 段階3・4: エンコーダ、デコーダ、貪欲法での文字起こし(英語・時刻なし)。`jfk.wav` の文字起こしが **whisper.cpp と完全に同じ**でした。30秒の音声で、エンコードは約3〜4秒、全体で約10秒です。

**まだできないこと(正直に)**: 確認したモデルは `ggml-tiny` だけです。量子化モデル(q4/q8 など)、英語以外、時刻つき出力、ビームサーチ、CLI・サーバー、GPU は未対応です。エンコーダの途中の値そのものの比較は未了で、いまは最終的な文字起こしの一致で確かめています。

**試し方(WSL / Linux)**: `bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>`(C++ 基準版は `tests/ref/build_cpp.sh` で作ります)。詳しくは [PORTING.md](PORTING.md)、決まりごとは [POLICY.md](POLICY.md)。

ライセンス: MIT(whisper.cpp 由来、(c) The ggml authors)。

## English
A step-by-step Rust port of [whisper.cpp](https://github.com/ggml-org/whisper.cpp) (MIT), a speech-to-text tool.

**What works today (2026-11-21, checked on the local PC, CPU)**
- Stage 1: turning audio into a mel spectrogram. **All 328,000 values are bit-identical** to the C++ version (with synthetic filters and with the real filters read from the model file).
- Stage 2: loading ggml model files. On the real `ggml-tiny` (167 tensors) the result matches an independently written Python reader exactly.
- Stages 3 and 4: encoder, decoder and greedy decoding (English, no timestamps). The transcript of `jfk.wav` is **identical to whisper.cpp**. For 30 seconds of audio, encoding takes about 3–4 s and the whole run about 10 s.

**What does not work yet (honestly)**: only `ggml-tiny` has been tested. Quantized models (q4/q8 and so on), languages other than English, timestamps, beam search, a CLI or server, and GPU are not supported. Intermediate encoder values have not been compared directly; for now we check the final transcript.

**Try it (WSL / Linux)**: `bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>` (build the C++ reference with `tests/ref/build_cpp.sh`). See [PORTING.md](PORTING.md) for details and [POLICY.md](POLICY.md) for the rules.

License: MIT (derived from whisper.cpp, (c) The ggml authors).

## 简体中文
这是一个把 [whisper.cpp](https://github.com/ggml-org/whisper.cpp)(MIT)逐步移植到 Rust 的项目,用于语音转文字。

**目前可以做到的(2026-11-21,在本地电脑的 CPU 上验证)**
- 阶段 1:把音频转换为梅尔频谱。与 C++ 版相比,**32.8 万个数值逐位完全一致**(使用合成滤波器和从模型文件读取的真实滤波器两种情况)。
- 阶段 2:读取 ggml 模型文件。对真实的 `ggml-tiny`(167 个张量),结果与另行编写的 Python 读取程序完全一致。
- 阶段 3 和 4:编码器、解码器和贪心解码(英语、不含时间戳)。`jfk.wav` 的转写结果与 whisper.cpp **完全相同**。处理 30 秒音频时,编码约需 3 至 4 秒,整个过程约 10 秒。

**尚未实现的部分(如实说明)**:目前只测试过 `ggml-tiny`。量化模型(q4/q8 等)、英语以外的语言、时间戳、束搜索、命令行和服务器、GPU 都还不支持。编码器的中间数值尚未直接对比,目前以最终转写结果是否一致来验证。

**试用方法(WSL / Linux)**:`bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>`(C++ 参考版用 `tests/ref/build_cpp.sh` 构建)。详情见 [PORTING.md](PORTING.md),规则见 [POLICY.md](POLICY.md)。

许可证:MIT(源自 whisper.cpp,(c) The ggml authors)。

## 繁體中文
這是一個把 [whisper.cpp](https://github.com/ggml-org/whisper.cpp)(MIT)逐步移植到 Rust 的專案,用於語音轉文字。

**目前可以做到的(2026-11-21,在本機電腦的 CPU 上驗證)**
- 階段 1:把音訊轉換為梅爾頻譜。與 C++ 版相比,**32.8 萬個數值逐位元完全一致**(使用合成濾波器與從模型檔讀取的真實濾波器兩種情況)。
- 階段 2:讀取 ggml 模型檔。對真實的 `ggml-tiny`(167 個張量),結果與另外撰寫的 Python 讀取程式完全一致。
- 階段 3 與 4:編碼器、解碼器與貪婪解碼(英語、不含時間戳記)。`jfk.wav` 的轉寫結果與 whisper.cpp **完全相同**。處理 30 秒音訊時,編碼約需 3 至 4 秒,整個流程約 10 秒。

**尚未實現的部分(如實說明)**:目前只測試過 `ggml-tiny`。量化模型(q4/q8 等)、英語以外的語言、時間戳記、束搜尋、命令列與伺服器、GPU 都還不支援。編碼器的中間數值尚未直接比對,目前以最終轉寫結果是否一致來驗證。

**試用方式(WSL / Linux)**:`bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>`(C++ 參考版請用 `tests/ref/build_cpp.sh` 建置)。詳情見 [PORTING.md](PORTING.md),規則見 [POLICY.md](POLICY.md)。

授權:MIT(源自 whisper.cpp,(c) The ggml authors)。

## 한국어
[whisper.cpp](https://github.com/ggml-org/whisper.cpp)(MIT)를 단계적으로 Rust로 옮기는 프로젝트이며, 음성을 글자로 바꾸는 도구입니다.

**지금 되는 것(2026-11-21, 로컬 PC의 CPU에서 확인)**
- 1단계: 음성을 멜 스펙트로그램으로 바꾸는 부분입니다. C++ 버전과 **32만 8천 개의 값이 비트 단위로 모두 같습니다**(합성 필터와 모델 파일에서 읽은 실제 필터 모두).
- 2단계: ggml 모델 파일 읽기입니다. 실제 `ggml-tiny`(텐서 167개)에서 따로 작성한 Python 읽기 프로그램과 결과가 완전히 일치합니다.
- 3·4단계: 인코더, 디코더, 탐욕적 디코딩(영어, 시간 정보 없음)입니다. `jfk.wav`의 받아쓰기 결과가 whisper.cpp와 **완전히 같습니다**. 30초 음성에서 인코딩은 약 3~4초, 전체는 약 10초가 걸립니다.

**아직 안 되는 것(솔직하게)**: 시험한 모델은 `ggml-tiny` 하나뿐입니다. 양자화 모델(q4/q8 등), 영어 이외의 언어, 시간 정보, 빔 서치, CLI와 서버, GPU는 지원하지 않습니다. 인코더 중간 값은 직접 비교하지 못했고, 지금은 최종 받아쓰기 결과가 같은지로 확인합니다.

**써 보기(WSL / Linux)**: `bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>`(C++ 기준 버전은 `tests/ref/build_cpp.sh`로 만듭니다). 자세한 내용은 [PORTING.md](PORTING.md), 규칙은 [POLICY.md](POLICY.md)를 보세요.

라이선스: MIT(whisper.cpp에서 파생, (c) The ggml authors).

## Deutsch
Eine schrittweise Rust-Portierung von [whisper.cpp](https://github.com/ggml-org/whisper.cpp) (MIT), einem Werkzeug zur Spracherkennung.

**Was heute funktioniert (21.11.2026, auf dem lokalen PC mit CPU geprüft)**
- Stufe 1: Umwandlung von Audio in ein Mel-Spektrogramm. **Alle 328.000 Werte stimmen bitgenau** mit der C++-Version überein (mit synthetischen Filtern und mit den echten Filtern aus der Modelldatei).
- Stufe 2: Laden von ggml-Modelldateien. Beim echten `ggml-tiny` (167 Tensoren) stimmt das Ergebnis exakt mit einem unabhängig geschriebenen Python-Leser überein.
- Stufen 3 und 4: Encoder, Decoder und Greedy-Dekodierung (Englisch, ohne Zeitstempel). Das Transkript von `jfk.wav` ist **identisch mit dem von whisper.cpp**. Bei 30 Sekunden Audio dauert das Encodieren etwa 3–4 s, der gesamte Lauf etwa 10 s.

**Was noch nicht geht (ehrlich gesagt)**: Getestet wurde nur `ggml-tiny`. Quantisierte Modelle (q4/q8 usw.), andere Sprachen als Englisch, Zeitstempel, Beam Search, CLI oder Server sowie GPU werden noch nicht unterstützt. Zwischenwerte des Encoders wurden nicht direkt verglichen; derzeit prüfen wir das fertige Transkript.

**Ausprobieren (WSL / Linux)**: `bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>` (die C++-Referenz wird mit `tests/ref/build_cpp.sh` gebaut). Details in [PORTING.md](PORTING.md), Regeln in [POLICY.md](POLICY.md).

Lizenz: MIT (abgeleitet von whisper.cpp, (c) The ggml authors).

## Français
Portage progressif en Rust de [whisper.cpp](https://github.com/ggml-org/whisper.cpp) (MIT), un outil de reconnaissance vocale.

**Ce qui fonctionne aujourd'hui (21/11/2026, vérifié sur le PC local, CPU)**
- Étape 1 : transformation de l'audio en spectrogramme de Mel. **Les 328 000 valeurs sont identiques au bit près** à celles de la version C++ (avec des filtres synthétiques et avec les vrais filtres lus dans le fichier du modèle).
- Étape 2 : chargement des fichiers de modèle ggml. Sur le vrai `ggml-tiny` (167 tenseurs), le résultat correspond exactement à celui d'un lecteur Python écrit séparément.
- Étapes 3 et 4 : encodeur, décodeur et décodage glouton (anglais, sans horodatage). La transcription de `jfk.wav` est **identique à celle de whisper.cpp**. Pour 30 secondes d'audio, l'encodage prend environ 3 à 4 s et l'ensemble environ 10 s.

**Ce qui ne fonctionne pas encore (en toute franchise)** : seul `ggml-tiny` a été testé. Les modèles quantifiés (q4/q8, etc.), les langues autres que l'anglais, les horodatages, la recherche par faisceau, la CLI ou le serveur et le GPU ne sont pas pris en charge. Les valeurs intermédiaires de l'encodeur n'ont pas été comparées directement ; pour l'instant, nous vérifions la transcription finale.

**Essayer (WSL / Linux)** : `bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>` (la référence C++ se compile avec `tests/ref/build_cpp.sh`). Détails dans [PORTING.md](PORTING.md), règles dans [POLICY.md](POLICY.md).

Licence : MIT (dérivée de whisper.cpp, (c) The ggml authors).

## Español
Port progresivo a Rust de [whisper.cpp](https://github.com/ggml-org/whisper.cpp) (MIT), una herramienta de reconocimiento de voz.

**Qué funciona hoy (21/11/2026, comprobado en el PC local, CPU)**
- Etapa 1: convertir el audio en un espectrograma de Mel. **Los 328 000 valores coinciden bit a bit** con la versión en C++ (con filtros sintéticos y con los filtros reales leídos del archivo del modelo).
- Etapa 2: cargar archivos de modelo ggml. Con el `ggml-tiny` real (167 tensores), el resultado coincide exactamente con el de un lector en Python escrito aparte.
- Etapas 3 y 4: codificador, decodificador y decodificación voraz (inglés, sin marcas de tiempo). La transcripción de `jfk.wav` es **idéntica a la de whisper.cpp**. Con 30 segundos de audio, la codificación tarda unos 3–4 s y el proceso completo unos 10 s.

**Lo que todavía no funciona (con franqueza)**: solo se ha probado `ggml-tiny`. No se admiten modelos cuantizados (q4/q8, etc.), idiomas distintos del inglés, marcas de tiempo, búsqueda en haz, CLI o servidor, ni GPU. Los valores intermedios del codificador no se han comparado directamente; por ahora comprobamos la transcripción final.

**Probarlo (WSL / Linux)**: `bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>` (la referencia en C++ se compila con `tests/ref/build_cpp.sh`). Detalles en [PORTING.md](PORTING.md) y reglas en [POLICY.md](POLICY.md).

Licencia: MIT (derivada de whisper.cpp, (c) The ggml authors).

## Português
Port gradual para Rust do [whisper.cpp](https://github.com/ggml-org/whisper.cpp) (MIT), uma ferramenta de reconhecimento de fala.

**O que funciona hoje (21/11/2026, verificado no PC local, CPU)**
- Etapa 1: transformar o áudio em um espectrograma de Mel. **Todos os 328 000 valores são idênticos bit a bit** aos da versão em C++ (com filtros sintéticos e com os filtros reais lidos do arquivo do modelo).
- Etapa 2: carregar arquivos de modelo ggml. Com o `ggml-tiny` real (167 tensores), o resultado coincide exatamente com o de um leitor em Python escrito separadamente.
- Etapas 3 e 4: codificador, decodificador e decodificação gulosa (inglês, sem marcas de tempo). A transcrição de `jfk.wav` é **idêntica à do whisper.cpp**. Com 30 segundos de áudio, a codificação leva cerca de 3 a 4 s e a execução completa cerca de 10 s.

**O que ainda não funciona (com franqueza)**: apenas o `ggml-tiny` foi testado. Modelos quantizados (q4/q8 etc.), idiomas além do inglês, marcas de tempo, busca em feixe, CLI ou servidor e GPU ainda não são suportados. Os valores intermediários do codificador não foram comparados diretamente; por enquanto verificamos a transcrição final.

**Experimente (WSL / Linux)**: `bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>` (a referência em C++ é compilada com `tests/ref/build_cpp.sh`). Detalhes em [PORTING.md](PORTING.md) e regras em [POLICY.md](POLICY.md).

Licença: MIT (derivada do whisper.cpp, (c) The ggml authors).

## Русский
Поэтапный перенос [whisper.cpp](https://github.com/ggml-org/whisper.cpp) (MIT) на Rust. Это инструмент распознавания речи.

**Что работает сейчас (21.11.2026, проверено на локальном ПК, CPU)**
- Этап 1: преобразование звука в мел-спектрограмму. **Все 328 000 значений совпадают с версией на C++ бит в бит** (и с синтетическими фильтрами, и с настоящими фильтрами из файла модели).
- Этап 2: чтение файлов моделей ggml. На настоящей модели `ggml-tiny` (167 тензоров) результат полностью совпадает с отдельно написанным читателем на Python.
- Этапы 3 и 4: кодировщик, декодировщик и жадное декодирование (английский, без временных меток). Расшифровка `jfk.wav` **полностью совпадает с whisper.cpp**. На 30 секундах звука кодирование занимает около 3–4 с, весь запуск — около 10 с.

**Чего пока нет (честно)**: проверена только модель `ggml-tiny`. Квантованные модели (q4/q8 и т. д.), языки кроме английского, временные метки, лучевой поиск, CLI и сервер, а также GPU пока не поддерживаются. Промежуточные значения кодировщика напрямую не сравнивались; сейчас мы проверяем итоговую расшифровку.

**Как попробовать (WSL / Linux)**: `bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>` (эталон на C++ собирается через `tests/ref/build_cpp.sh`). Подробности — в [PORTING.md](PORTING.md), правила — в [POLICY.md](POLICY.md).

Лицензия: MIT (производная от whisper.cpp, (c) The ggml authors).

## العربية
<div dir="rtl">

نقلٌ تدريجي إلى لغة Rust لمشروع [whisper.cpp](https://github.com/ggml-org/whisper.cpp) (بترخيص MIT)، وهو أداة للتعرّف على الكلام وتحويله إلى نص.

**ما يعمل اليوم (21‏/11‏/2026، جرى التحقق منه على الحاسوب المحلي باستخدام المعالج CPU)**
- المرحلة 1: تحويل الصوت إلى مخطط طيفي بمقياس ميل. **جميع القيم البالغ عددها 328000 مطابقة بتًّا ببت** لإصدار C++ (مع مرشِّحات اصطناعية ومع المرشِّحات الحقيقية المقروءة من ملف النموذج).
- المرحلة 2: قراءة ملفات نماذج ggml. على النموذج الحقيقي `ggml-tiny` (167 موتّرًا) جاءت النتيجة مطابقة تمامًا لقارئ مكتوب بلغة Python بصورة مستقلة.
- المرحلتان 3 و4: المرمِّز وفاكّ الترميز والفكّ الجشع (الإنجليزية، دون طوابع زمنية). جاء النص المفرَّغ لملف `jfk.wav` **مطابقًا تمامًا لما يُنتجه whisper.cpp**. وفي مقطع صوتي مدته 30 ثانية يستغرق الترميز نحو 3 إلى 4 ثوانٍ، وتستغرق العملية كاملةً نحو 10 ثوانٍ.

**ما لا يعمل بعد (بصراحة)**: لم يُختبر سوى النموذج `ggml-tiny`. النماذج المكمَّمة (q4 وq8 وغيرهما) واللغات غير الإنجليزية والطوابع الزمنية والبحث بالحزمة وواجهة سطر الأوامر والخادم ووحدة معالجة الرسوميات GPU غير مدعومة حاليًا. ولم تُقارَن القيم الوسيطة للمرمِّز مباشرةً؛ ونكتفي الآن بالتحقق من تطابق النص النهائي.

**للتجربة (WSL / Linux)**: ‏`bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>` (يُبنى مرجع C++ بالأمر `tests/ref/build_cpp.sh`). التفاصيل في [PORTING.md](PORTING.md) والقواعد في [POLICY.md](POLICY.md).

الترخيص: MIT (مشتق من whisper.cpp، ‏(c) The ggml authors).

</div>

## فارسی
<div dir="rtl">

انتقال گام‌به‌گام [whisper.cpp](https://github.com/ggml-org/whisper.cpp) (با مجوز MIT) به زبان Rust؛ ابزاری برای تشخیص گفتار و تبدیل آن به متن.

**آنچه امروز کار می‌کند (۲۱ نوامبر ۲۰۲۶، روی رایانهٔ محلی و با پردازندهٔ CPU بررسی شده)**
- مرحلهٔ ۱: تبدیل صدا به طیف‌نگار مل. **هر ۳۲۸٬۰۰۰ مقدار بیت‌به‌بیت با نسخهٔ C++ یکسان است** (هم با فیلترهای ساختگی و هم با فیلترهای واقعیِ خوانده‌شده از فایل مدل).
- مرحلهٔ ۲: خواندن فایل‌های مدل ggml. روی مدل واقعی `ggml-tiny` (۱۶۷ تانسور) نتیجه دقیقاً با یک خواننده‌ٔ جداگانهٔ نوشته‌شده با Python برابر است.
- مرحله‌های ۳ و ۴: رمزگذار، رمزگشا و رمزگشایی حریصانه (انگلیسی، بدون برچسب زمانی). متن پیاده‌شدهٔ `jfk.wav` **دقیقاً با whisper.cpp یکسان** است. برای ۳۰ ثانیه صدا، رمزگذاری حدود ۳ تا ۴ ثانیه و کل اجرا حدود ۱۰ ثانیه طول می‌کشد.

**آنچه هنوز کار نمی‌کند (صادقانه)**: فقط `ggml-tiny` آزمایش شده است. مدل‌های کوانتیزه (q4 و q8 و مانند آن)، زبان‌هایی جز انگلیسی، برچسب زمانی، جستجوی پرتو، رابط خط فرمان و سرور و نیز GPU هنوز پشتیبانی نمی‌شوند. مقدارهای میانیِ رمزگذار مستقیماً مقایسه نشده‌اند؛ فعلاً تطابق متن نهایی را بررسی می‌کنیم.

**برای امتحان (WSL / Linux)**: ‏`bash tests/ref/transcribe_run.sh <ggml-tiny.bin> <jfk.wav>` (مرجع C++ با `tests/ref/build_cpp.sh` ساخته می‌شود). جزئیات در [PORTING.md](PORTING.md) و قواعد در [POLICY.md](POLICY.md) آمده است.

مجوز: MIT (مشتق از whisper.cpp، ‏(c) The ggml authors).

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