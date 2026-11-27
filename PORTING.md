# PORTING.md — rs-whisper 移植の記録 / Porting log

元: [whisper.cpp](https://github.com/ggml-org/whisper.cpp)(MIT)。方針は [`POLICY.md`](POLICY.md)、AI 向けの決まりは [`CLAUDE.md`](CLAUDE.md)。 全12言語・AI翻訳(ネイティブ検証前)。

## 日本語
**段階と結果(2026-11-21、すべて main に push 済み)**
| 段階 | 内容 | 確認結果 |
|---|---|---|
| 1 | ログメル・スペクトログラム | 328000/328000 が C++ と完全一致(最大差 0) |
| 2 | ggml モデルの読み込み(f32/f16/q4_0〜q8_0 の配置) | 実 `ggml-tiny.bin`(167テンソル)が独立した Python 読み取りと一致 |
| 3 | エンコーダ(conv×2、位置埋め込み、4ブロック) | 最終文字起こしで間接的に確認 |
| 4 | デコーダ+貪欲法(英語・時刻なし) | tiny + jfk.wav の文字起こしが whisper.cpp と一致(エンコード約3〜4秒、全体約8〜10秒) |

**再現**: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh`(基準 `whisper-cli` を作る)→ `…/transcribe_run.sh <モデル> <wav>`。
**正直な限界**: tiny のみ確認。f32/f16 の重みだけ(量子化は読めるが推論は未対応)。エンコーダの途中値は数値比較していない。GELU は tanh 近似。
**未了(順に)**: 1) エンコーダ途中値の数値比較 2) 量子化重みの推論 3) ビームサーチ・時刻付き・多言語 4) CLI と Connect サーバー 5) 大きなモデル・GPU。

## English
**Stages and results (2026-11-21, all pushed to main)**
| Stage | What | Result |
|---|---|---|
| 1 | log-mel spectrogram | 328000/328000 bit-identical to C++ (max diff 0) |
| 2 | ggml model loader (f32/f16/q4_0–q8_0 layouts) | real `ggml-tiny.bin` (167 tensors) matches an independent Python reader |
| 3 | encoder (2 convs, positional embedding, 4 blocks) | checked indirectly through the final transcript |
| 4 | decoder + greedy search (English, no timestamps) | tiny + jfk.wav transcript identical to whisper.cpp (encode ~3–4 s, whole run ~8–10 s) |

**Reproduce**: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh` (builds the reference `whisper-cli`), then `…/transcribe_run.sh <model> <wav>`.
**Honest limits**: only tiny tested; f32/f16 weights only (quantized types parse but cannot run inference); encoder intermediates not compared numerically; GELU is the tanh approximation.
**Open (in order)**: 1) numeric comparison of encoder intermediates 2) quantized inference 3) beam search, timestamps, multilingual 4) CLI and Connect server 5) larger models, GPU.

## 简体中文
**阶段与结果(2026-11-21,均已推送到 main)**
| 阶段 | 内容 | 验证结果 |
|---|---|---|
| 1 | 对数梅尔频谱 | 328000/328000 与 C++ 完全一致(最大差 0) |
| 2 | ggml 模型读取(f32/f16/q4_0–q8_0 布局) | 真实 `ggml-tiny.bin`(167 个张量)与独立的 Python 读取结果一致 |
| 3 | 编码器(2 个卷积、位置嵌入、4 个块) | 通过最终转写间接验证 |
| 4 | 解码器+贪心搜索(英语、无时间戳) | tiny + jfk.wav 的转写与 whisper.cpp 一致(编码约 3–4 秒,整体约 8–10 秒) |

**复现**:`wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh`(构建基准 `whisper-cli`),然后运行 `…/transcribe_run.sh <模型> <wav>`。
**如实说明的局限**:只验证了 tiny;仅支持 f32/f16 权重(量化类型能解析但不能推理);编码器中间值未做数值比较;GELU 为 tanh 近似。
**未完成(按顺序)**:1) 编码器中间值的数值比较 2) 量化权重推理 3) 束搜索、时间戳、多语言 4) CLI 与 Connect 服务器 5) 更大的模型、GPU。

## 繁體中文
**階段與結果(2026-11-21,均已推送到 main)**
| 階段 | 內容 | 驗證結果 |
|---|---|---|
| 1 | 對數梅爾頻譜 | 328000/328000 與 C++ 完全一致(最大差 0) |
| 2 | ggml 模型讀取(f32/f16/q4_0–q8_0 佈局) | 真實 `ggml-tiny.bin`(167 個張量)與獨立的 Python 讀取結果一致 |
| 3 | 編碼器(2 個卷積、位置嵌入、4 個區塊) | 透過最終轉寫間接驗證 |
| 4 | 解碼器+貪婪搜尋(英語、無時間戳) | tiny + jfk.wav 的轉寫與 whisper.cpp 一致(編碼約 3–4 秒,整體約 8–10 秒) |

**重現**:`wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh`(建置基準 `whisper-cli`),再執行 `…/transcribe_run.sh <模型> <wav>`。
**如實說明的限制**:只驗證了 tiny;僅支援 f32/f16 權重(量化類型可解析但無法推論);編碼器中間值未做數值比對;GELU 為 tanh 近似。
**尚待完成(依序)**:1) 編碼器中間值的數值比對 2) 量化權重推論 3) 束搜尋、時間戳、多語言 4) CLI 與 Connect 伺服器 5) 更大的模型、GPU。

## 한국어
**단계와 결과(2026-11-21, 모두 main에 push 완료)**
| 단계 | 내용 | 확인 결과 |
|---|---|---|
| 1 | 로그 멜 스펙트로그램 | 328000/328000이 C++와 완전 일치(최대 차이 0) |
| 2 | ggml 모델 읽기(f32/f16/q4_0–q8_0 배치) | 실제 `ggml-tiny.bin`(텐서 167개)이 독립된 Python 읽기와 일치 |
| 3 | 인코더(컨볼루션 2개, 위치 임베딩, 블록 4개) | 최종 전사를 통해 간접 확인 |
| 4 | 디코더 + 탐욕 탐색(영어, 타임스탬프 없음) | tiny + jfk.wav 전사가 whisper.cpp와 일치(인코딩 약 3~4초, 전체 약 8~10초) |

**재현**: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh`(기준 `whisper-cli` 빌드) 후 `…/transcribe_run.sh <모델> <wav>`.
**솔직한 한계**: tiny만 확인했습니다. f32/f16 가중치만 지원합니다(양자화 형식은 읽지만 추론은 못 합니다). 인코더 중간값은 수치 비교를 하지 않았고, GELU는 tanh 근사입니다.
**미완료(순서대로)**: 1) 인코더 중간값 수치 비교 2) 양자화 가중치 추론 3) 빔 서치·타임스탬프·다국어 4) CLI와 Connect 서버 5) 더 큰 모델·GPU.

## Deutsch
**Stufen und Ergebnisse (2026-11-21, alles auf main gepusht)**
| Stufe | Inhalt | Ergebnis |
|---|---|---|
| 1 | Log-Mel-Spektrogramm | 328000/328000 bitgleich mit C++ (maximale Abweichung 0) |
| 2 | ggml-Modell laden (Layouts f32/f16/q4_0–q8_0) | echtes `ggml-tiny.bin` (167 Tensoren) stimmt mit einem unabhängigen Python-Leser überein |
| 3 | Encoder (2 Faltungen, Positionseinbettung, 4 Blöcke) | indirekt über das fertige Transkript geprüft |
| 4 | Decoder + Greedy-Suche (Englisch, ohne Zeitstempel) | Transkript von tiny + jfk.wav identisch mit whisper.cpp (Encoder ca. 3–4 s, gesamt ca. 8–10 s) |

**Nachstellen**: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh` (baut das Referenz-`whisper-cli`), danach `…/transcribe_run.sh <Modell> <wav>`.
**Ehrliche Grenzen**: nur tiny getestet; nur f32/f16-Gewichte (quantisierte Typen werden gelesen, aber nicht für Inferenz genutzt); Encoder-Zwischenwerte nicht numerisch verglichen; GELU ist die tanh-Näherung.
**Offen (der Reihe nach)**: 1) numerischer Vergleich der Encoder-Zwischenwerte 2) Inferenz mit quantisierten Gewichten 3) Beam Search, Zeitstempel, Mehrsprachigkeit 4) CLI und Connect-Server 5) größere Modelle, GPU.

## Français
**Étapes et résultats (2026-11-21, tout est poussé sur main)**
| Étape | Contenu | Résultat |
|---|---|---|
| 1 | spectrogramme log-mel | 328000/328000 identique bit à bit à C++ (écart max. 0) |
| 2 | chargement du modèle ggml (formats f32/f16/q4_0–q8_0) | le vrai `ggml-tiny.bin` (167 tenseurs) correspond à un lecteur Python indépendant |
| 3 | encodeur (2 convolutions, plongement de position, 4 blocs) | vérifié indirectement par la transcription finale |
| 4 | décodeur + recherche gloutonne (anglais, sans horodatage) | transcription de tiny + jfk.wav identique à whisper.cpp (encodeur ≈ 3–4 s, total ≈ 8–10 s) |

**Reproduire** : `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh` (construit le `whisper-cli` de référence), puis `…/transcribe_run.sh <modèle> <wav>`.
**Limites, franchement** : seul tiny a été testé ; uniquement des poids f32/f16 (les types quantifiés sont lus mais pas utilisés en inférence) ; valeurs intermédiaires de l'encodeur non comparées numériquement ; GELU en approximation tanh.
**À faire (dans l'ordre)** : 1) comparaison numérique des valeurs intermédiaires de l'encodeur 2) inférence avec poids quantifiés 3) recherche par faisceau, horodatage, multilingue 4) CLI et serveur Connect 5) modèles plus grands, GPU.

## Español
**Etapas y resultados (2026-11-21, todo subido a main)**
| Etapa | Contenido | Resultado |
|---|---|---|
| 1 | espectrograma log-mel | 328000/328000 idéntico bit a bit a C++ (diferencia máxima 0) |
| 2 | carga del modelo ggml (formatos f32/f16/q4_0–q8_0) | el `ggml-tiny.bin` real (167 tensores) coincide con un lector Python independiente |
| 3 | codificador (2 convoluciones, incrustación posicional, 4 bloques) | comprobado indirectamente con la transcripción final |
| 4 | decodificador + búsqueda voraz (inglés, sin marcas de tiempo) | la transcripción de tiny + jfk.wav es idéntica a la de whisper.cpp (codificador ≈ 3–4 s, total ≈ 8–10 s) |

**Reproducir**: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh` (compila el `whisper-cli` de referencia) y luego `…/transcribe_run.sh <modelo> <wav>`.
**Límites, con franqueza**: solo se probó tiny; solo pesos f32/f16 (los tipos cuantizados se leen, pero no se usan para inferir); los valores intermedios del codificador no se compararon numéricamente; GELU es la aproximación tanh.
**Pendiente (en orden)**: 1) comparación numérica de los valores intermedios del codificador 2) inferencia con pesos cuantizados 3) búsqueda por haz, marcas de tiempo, multilingüe 4) CLI y servidor Connect 5) modelos más grandes, GPU.

## Português
**Etapas e resultados (2026-11-21, tudo enviado para a main)**
| Etapa | Conteúdo | Resultado |
|---|---|---|
| 1 | espectrograma log-mel | 328000/328000 idêntico bit a bit ao C++ (diferença máxima 0) |
| 2 | carregamento do modelo ggml (formatos f32/f16/q4_0–q8_0) | o `ggml-tiny.bin` real (167 tensores) coincide com um leitor Python independente |
| 3 | codificador (2 convoluções, incorporação posicional, 4 blocos) | verificado indiretamente pela transcrição final |
| 4 | decodificador + busca gulosa (inglês, sem marcas de tempo) | transcrição de tiny + jfk.wav idêntica à do whisper.cpp (codificador ≈ 3–4 s, total ≈ 8–10 s) |

**Reproduzir**: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh` (compila o `whisper-cli` de referência) e depois `…/transcribe_run.sh <modelo> <wav>`.
**Limites, com franqueza**: só o tiny foi testado; apenas pesos f32/f16 (os tipos quantizados são lidos, mas não usados na inferência); valores intermediários do codificador não foram comparados numericamente; GELU é a aproximação tanh.
**Pendente (em ordem)**: 1) comparação numérica dos valores intermediários do codificador 2) inferência com pesos quantizados 3) busca em feixe, marcas de tempo, multilíngue 4) CLI e servidor Connect 5) modelos maiores, GPU.

## Русский
**Этапы и результаты (2026-11-21, всё отправлено в main)**
| Этап | Что сделано | Результат |
|---|---|---|
| 1 | лог-мел-спектрограмма | 328000/328000 совпадает с C++ побитно (максимальная разница 0) |
| 2 | загрузка модели ggml (форматы f32/f16/q4_0–q8_0) | настоящий `ggml-tiny.bin` (167 тензоров) совпал с независимым чтением на Python |
| 3 | энкодер (2 свёртки, позиционное вложение, 4 блока) | проверен косвенно по итоговой транскрипции |
| 4 | декодер + жадный поиск (английский, без временных меток) | транскрипция tiny + jfk.wav идентична whisper.cpp (энкодер ≈ 3–4 с, всего ≈ 8–10 с) |

**Как повторить**: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh` (собирает эталонный `whisper-cli`), затем `…/transcribe_run.sh <модель> <wav>`.
**Честные ограничения**: проверен только tiny; только веса f32/f16 (квантованные типы читаются, но для вывода не используются); промежуточные значения энкодера численно не сравнивались; GELU — приближение tanh.
**Осталось (по порядку)**: 1) численное сравнение промежуточных значений энкодера 2) вывод на квантованных весах 3) лучевой поиск, временные метки, многоязычность 4) CLI и сервер Connect 5) более крупные модели, GPU.

## العربية
<div dir="rtl">

**المراحل والنتائج (2026-11-21، وكلها مدفوعة إلى main)**
| المرحلة | المحتوى | نتيجة التحقق |
|---|---|---|
| 1 | طيف ميل اللوغاريتمي | 328000/328000 مطابق بتًّا ببت لـ C++ (أقصى فرق 0) |
| 2 | تحميل نموذج ggml (تخطيطات f32/f16/q4_0–q8_0) | الملف الحقيقي `ggml-tiny.bin` (167 موتّرًا) يطابق قارئ Python مستقلًّا |
| 3 | المشفّر (التفافان، تضمين موضعي، 4 كتل) | تم التحقق منه بصورة غير مباشرة عبر النص النهائي |
| 4 | المفكّك + البحث الجشع (إنجليزي، بلا طوابع زمنية) | نص tiny مع jfk.wav مطابق لنص whisper.cpp (المشفّر نحو 3–4 ثوانٍ، والكل نحو 8–10 ثوانٍ) |

**إعادة التجربة**: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh` (يبني `whisper-cli` المرجعي)، ثم `…/transcribe_run.sh <النموذج> <wav>`.
**حدود بصراحة**: لم يُختبر إلا tiny؛ تُدعم أوزان f32/f16 فقط (تُقرأ الأنواع المكمّمة لكن لا تُستخدم في الاستدلال)؛ لم تُقارَن القيم الوسيطة في المشفّر عدديًّا؛ وGELU تقريب tanh.
**المتبقي (بالترتيب)**: 1) المقارنة العددية للقيم الوسيطة في المشفّر 2) الاستدلال بالأوزان المكمّمة 3) البحث الشعاعي والطوابع الزمنية وتعدد اللغات 4) CLI وخادم Connect 5) نماذج أكبر وGPU.

</div>

## فارسی
<div dir="rtl">

**مرحله‌ها و نتیجه‌ها (۲۰۲۶-۱۱-۲۱، همه به main فرستاده شده)**
| مرحله | محتوا | نتیجهٔ بررسی |
|---|---|---|
| ۱ | طیف‌نگار لگاریتمی مل | ۳۲۸۰۰۰ از ۳۲۸۰۰۰ بیت‌به‌بیت با C++‎ یکسان (بیشینهٔ اختلاف ۰) |
| ۲ | بارگذاری مدل ggml (چیدمان‌های f32/f16/q4_0–q8_0) | فایل واقعی `ggml-tiny.bin` (۱۶۷ تانسور) با خوانندهٔ مستقل Python یکسان است |
| ۳ | رمزگذار (۲ کانولوشن، تعبیهٔ مکانی، ۴ بلوک) | به‌طور غیرمستقیم از راه متن نهایی بررسی شد |
| ۴ | رمزگشا + جستجوی حریصانه (انگلیسی، بدون برچسب زمانی) | رونویسی tiny با jfk.wav با whisper.cpp یکسان است (رمزگذار حدود ۳ تا ۴ ثانیه، کل حدود ۸ تا ۱۰ ثانیه) |

**بازتولید**: `wsl -d Ubuntu-24.04 -- bash /mnt/f/rs-whisper/tests/ref/build_cpp.sh` (`whisper-cli` مرجع را می‌سازد) و سپس `…/transcribe_run.sh <مدل> <wav>`.
**محدودیت‌ها، صادقانه**: فقط tiny آزموده شده؛ فقط وزن‌های f32/f16 (نوع‌های کوانتیزه خوانده می‌شوند ولی در استنتاج به کار نمی‌روند)؛ مقدارهای میانی رمزگذار به‌صورت عددی مقایسه نشده‌اند؛ GELU تقریب tanh است.
**باقی‌مانده (به ترتیب)**: ۱) مقایسهٔ عددی مقدارهای میانی رمزگذار ۲) استنتاج با وزن‌های کوانتیزه ۳) جستجوی پرتویی، برچسب زمانی، چندزبانه ۴) CLI و سرور Connect ۵) مدل‌های بزرگ‌تر و GPU.

</div>

## 2026-11-23 追記: 常駐の区間処理(rs-whisper-stream)と重複除去(日本語)
- `rs-whisper-stream`(`src/bin/stream.rs`)を追加: モデルを1回だけ読み込み(常駐)、無音(0.3秒以上)で区間に切り、最小区間長(既定2.5秒)・最大8秒、前後0.3秒の重なりを付け、区間の長さ+1秒の窓(`audio_ctx`)で認識する。区間のつなぎは `src/merge.rs` の `append_dedup` が、境目で重複した語(最大8語、大文字小文字・句読点を無視)を取り除く。単体テスト3件成功。
- 実測(jfk.wav 11秒、tiny、CPU、WSL): 最小区間2.5秒で2区間(0〜8.2秒=3.3秒、7.6〜11秒=1.9秒で処理)、合計約5.2秒。結果は「And so my fellow Americans ask not what your country can do for you. ask what you can do for your country. Ask」で、前半・後半とも正しい。最小区間1.0秒だと「as not. As」と誤認識したので、短く切りすぎない。
- 字幕の遅れの目安: 区間が閉じてから約2〜3.5秒+翻訳約1秒。**区間の長さ(最大8秒)ぶんの待ちは残る**ので、話し始めから字幕までは最悪10秒前後。
- **未確認/限界**: 末尾に余計な「Ask」が付く(終端の重なり部分の幻覚。除去は未実装)。単語境目の重なりが認識結果でずれると重複を取り切れない。1つの話者・きれいな音声のみで確認(雑音・複数話者は未確認)。常駐は1プロセス内のループで、他プロセスから呼ぶ口(Connect)は未実装。

## 2026-11-23 追記: 語の時刻(交差注意DTW)と実時刻でのバッファ切り捨て(日本語)
- `rs-whisper` に `Whisper::token_times`(交差注意の整列ヘッド(tiny多言語: 2-2, 3-0, 3-2, 3-3, 3-4, 3-5)を平均→列ごとに正規化→時間方向の中央値フィルター→DTW)と `transcribe_segments`(タイムスタンプトークンによる区間時刻)、確認用 `rs-whisper-ts` を追加。
- 実測(jfk.wav 11秒、tiny、CPU): タイムスタンプトークンは全体を1区間(0〜10.56秒)にするだけで、語単位の切り捨てには粗すぎた。DTWの語時刻は And=0.00, so=0.70, my=1.08, fellow=1.32, Americans=1.70, ask=2.28, not=3.86, ... ask=8.02, ... country=10.04 と単調で妥当(目視。正解時刻との照合は未実施)。
- `rs-whisper-la` の切り捨てを、有声率による位置の推定から、この語時刻(最初の未確定語の開始−0.15秒で切る)に置き換えた。バッファ上限5秒で、以前出た「ask ask」の重複は出なくなり、全文が正しく確定: `And so my fellow Americans ask not what your country can do for you ask what you can do for your country.` 計算は約15秒(切り捨てなしは約15〜20秒、環境の負荷で揺れる)。試験4件成功。
- 終端の余分な語(「Ask」)は、最終ステップにも drop_dangling を適用して除去(限界: 句点の直後の本当の1語発話も落ちる)。
- **未確認/限界**: 整列ヘッドは tiny 多言語の既定値(他モデルは別の組。tiny.en 等は未対応)。語時刻の正解との照合、長い音声、base以上のモデルは未測定。遅れは依然として約3〜4秒+処理時間で、論文の約3.3秒には未達。`open-tv-chat` へのマイク入力の組み込みは未了。

## 2026-11-23 Update: word times (cross-attention DTW) and trimming by real times (English)
- Added `Whisper::token_times` (average of the cross-attention alignment heads of multilingual tiny: 2-2, 3-0, 3-2, 3-3, 3-4, 3-5; per-frame normalisation; median filter; DTW), `transcribe_segments` (segment times from timestamp tokens) and the check tool `rs-whisper-ts`.
- Measured (11 s `jfk.wav`, tiny, CPU): timestamp tokens give a single segment (0 to 10.56 s), too coarse for word-level trimming. DTW word times are monotone and plausible by eye (And=0.00, so=0.70, ... ask=8.02, country=10.04); not compared against ground truth.
- `rs-whisper-la` now cuts the buffer 0.15 s before the first non-final word using these times instead of the voiced-ratio estimate. With a 5 s buffer limit the earlier "ask ask" duplicate is gone and the full text is correct. Compute about 15 s (20 s without trimming; noisy). 4 tests pass.
- A stray trailing word ("Ask") is removed by also applying `drop_dangling` on the last step (limit: a genuine one-word utterance right after a sentence end is dropped too).
- **Unverified/limits**: alignment heads are for multilingual tiny only (tiny.en and larger models not handled); no ground-truth check, long audio or base-or-larger models yet; delay is still about 3-4 s plus processing, short of the paper's 3.3 s; microphone integration into `open-tv-chat` is open.

## Versionless API の方針 / Versionless API policy (2026-11-25)
**日本語**: このリポジトリのAPIは **Versionless**(URL・パッケージ名に `v1` 等を付けず、互換性を壊さず足すだけで進化させる)。欄は足すだけ・足す欄は省略可能・知らない欄は無視・名前変更は新名を足して旧を非推奨に(期限は既定で6か月以上、使用0を確認してから削除)・enumは未知の値を許容・安全上の緊急修正は例外。既存の `.v1` は呼び出し側を壊さないよう当面残し、版なしの別名を追加して段階的に畳む。詳細・出典は [`runo/API-POLICY.md`](https://github.com/aon-co-jp/runo/blob/main/API-POLICY.md) 末尾。適用はこれから順に行う。
**English**: This repository's APIs are **Versionless**: no `v1` in URLs or package names; evolve by adding only, without breaking compatibility. Only add fields, new fields are optional, ignore unknown fields, rename by adding the new name and deprecating the old (default deadline 6 months or more, remove only after confirming zero use), tolerate unknown enum values, emergency security fixes are the exception. Existing `.v1` names stay for now so callers do not break; a versionless alias is added and `.v1` is retired in stages. Details and sources: the end of [`runo/API-POLICY.md`](https://github.com/aon-co-jp/runo/blob/main/API-POLICY.md). Rollout is step by step.

## API 方針: REST 全廃・gRPC / Connect へ
**日本語**: このリポジトリでは **REST API を新規に作らない**。ブラウザ・`curl` から呼ぶ API は **Connect の単純呼び出し**(`POST /<パッケージ>.<Service>/<Method>`、`content-type: application/json`)、サービス間は **gRPC / gRPC-web**。GraphQL は読み取り専用。既存の REST は触るときに移し、移したら旧経路は消す。JSON は RS-JSON(`from_str_secure` / `parse_secure`)で読む。例外は相手の規格が形を決めるもの(ACME、OAuth/OIDC のコールバック、第三者の Webhook、ヘルスチェック、静的ファイル、OTLP)のみ。迷ったら Connect。正本は [`runo/API-POLICY.md`](https://github.com/aon-co-jp/runo/blob/main/API-POLICY.md)。
**English**: This repository **does not add new REST APIs**. APIs called from browsers or `curl` use **Connect unary calls** (`POST /<package>.<Service>/<Method>`, `content-type: application/json`); service-to-service calls use **gRPC / gRPC-web**. GraphQL is read-only. Existing REST is migrated when touched and the old route is removed. Read JSON with RS-JSON (`from_str_secure` / `parse_secure`). Exceptions are only shapes dictated by someone else's standard (ACME, OAuth/OIDC callbacks, third-party webhooks, health checks, static files, OTLP). When in doubt, use Connect. Source of truth: the link above.

## 完成まで作り切る方針 / Finish everything policy (2026-11-27)
**日本語**: 全リポジトリで、**未確認は自動で確認し、未実施は実施し、未了は完成させる**(確認を求めず進める)。使用頻度が低いと判断される機能や、**1から作り直す大規模開発になる内容でも、やめずに最後まで開発・TEST・DEBUGする**(別の方式で済ませない、後回しにしない)。進め方: 調査(英語・日本語の検索とGitHub) → 設計 → 実装 → 実機/実データで確認 → 失敗は原因を直して再試験 → README・CLAUDE・PORTING・POLICY を日英で更新 → push。「できた」と書くのは実測した範囲だけ、確認できなかった所は「未確認」と正直に書く。例外(確認を取る): 元に戻せない削除、秘密の公開、所有者の認証・決済・実機操作が要るもの、利用者のシステム設定の変更。
**English**: In every repository, **verify what is unverified, do what is not yet done, and finish what is unfinished** automatically, without asking. Even features that look rarely used, or work that means **a large rewrite from scratch, are developed, tested and debugged to the end** (no substituting another approach, no postponing). Method: research (English and Japanese search plus GitHub) -> design -> implement -> verify on real devices or data -> fix causes and retest on failure -> update README, CLAUDE, PORTING and POLICY in Japanese and English -> push. Write "done" only for what was measured, and say "unverified" honestly for the rest. Exceptions that still need confirmation: irreversible deletions, publishing secrets, anything needing the owner's authentication, payment or real-device actions, and changes to the user's system settings.