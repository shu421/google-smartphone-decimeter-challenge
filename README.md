# outdoor
<img width="791" alt="スクリーンショット 2021-05-30 17 26 54" src="https://user-images.githubusercontent.com/71954051/120097591-3d0fb380-c16c-11eb-8295-538cc14ed742.png">

# Overview
不意にポットホールなどの道路障害物にぶつかったことはありませんか？また、ナビゲーションアプリで、より正確な位置情報や車線レベルの精度が得られたらと思ったことはありませんか？これらの機能やその他の新しい機能は、スマートフォンの測位サービスによって実現されています。機械学習と高精度GNSSアルゴリズムにより、この精度が向上し、何十億人ものAndroid携帯電話ユーザーに、よりきめ細かな測位体験を提供できるようになると期待されています。



全地球測位衛星システム（GNSS）は、生の信号を提供し、GPSチップセットはそれを使って位置を計算します。現在の携帯電話では、3〜5メートルの位置精度しか得られません。これは多くの場合、便利ではありますが、"ビビり "の原因となります。多くのユースケースでは、その結果は、信頼できるほど細かくも安定してもいません。

Android GPSチームが主催するこのコンテストは、ION GNSS+ 2021 Conferenceで発表されます。彼らは、スマートフォンのGNSS測位精度の研究を進め、人々が身の回りの世界をよりよくナビゲートできるようにすることを目指しています。

このコンペティションでは、ホストチームが所有するAndroid携帯電話から収集したデータを使用して、可能であれば10cm、さらにはcm単位の分解能で位置を計算します。また、正確なグラウンドトゥルース、生のGPS測定値、近隣のGPSステーションからのアシスタンスデータを利用して、応募作品のトレーニングとテストを行います。

成功すれば、より正確な位置情報を得ることができ、より細かい人間の行動の地理空間情報と、より細かい粒度のモバイルインターネットとの橋渡しをすることができます。モバイルユーザーは、より良い車線レベルの座標を得て、ロケーションベースのゲームの経験を強化し、交通安全上の問題の位置をより具体的に把握することができます。さらには、行きたい場所に簡単に行けるようになったことに気づくかもしれません。



# Evaluation
提出されたデータは、50パーセンタイルと95パーセンタイルの距離誤差の平均値で採点されます。すべての電話機のすべての millisSinceGpsEpoch において、予測された緯度/経度とグランドトゥルースの緯度/経度との間の水平方向の距離（メートル）が計算されます。これらの距離誤差は分布を形成し、そこから50パーセンタイル誤差と95パーセンタイル誤差が計算されます（つまり、95パーセンタイル誤差は、距離誤差の95%が小さくなる値（メートル単位）です）。次に、50パーセンタイルと95パーセンタイルの誤差を各電話機ごとに平均化します。最後に，これらの平均化された値の平均値が，テストセットのすべての電話機について計算される．

提出ファイル
テストセット内の各電話機とmillisSinceGpsEpochについて、緯度と経度を予測する必要があります。少なくとも4つの有効なGNSS信号があるすべてのタイムスタンプについて、推定値を提供する必要があります。提出ファイルのサンプルには、これらのタイムスタンプのリストが含まれています。投稿ファイルにはヘッダーを含め、以下の形式で作成してください。

phone,millisSinceGpsEpoch,latDeg,lngDeg
2020-05-15-US-MTV-1_Pixel4,1273608785432,53.599227001298125,-2.4339795741464334
2020-05-15-US-MTV-1_Pixel4,1273608786432,53.599227001298125,-2.4339795741464334
2020-05-15-US-MTV-1_Pixel4,1273608787432,53.599227001298125,-2.4339795741464334    



# Data Description
このチャレンジでは、GPS衛星からの信号、加速度計の測定値、ジャイロスコープの測定値など、携帯電話の位置を決定するのに役立つさまざまな機器からのデータを提供します。

この課題では、車線レベルのマッピングなどの後処理に重点を置いて設計されているため、将来的にはルート上のデータを利用して、可能な限り正確な位置を生成することができます。また、複数の機種で構成される路線も多いため、隣接する機種の情報を利用して推定を行うこともできます。一般的なGNSS測位アルゴリズムの開発を促進するため、携帯電話内のGPSチップセットの位置情報は提供されません。これは、携帯電話のモデルなどによって異なるメーカー独自のアルゴリズムに基づいているためです。

データ収集プロセスの詳細については、本稿をご参照ください。このデータセット／課題に基づいて作品を発表する場合は、コンペティションルールに基づいて適切に引用してください。

- ファイル名
[train]/[drive_id]/[phone_name]/ground_truth.csv - トレーニングセットでのみ提供されます。予想されるタイムスタンプでの参照場所。
- train/test]/[drive_id]/[phone_name]/supplemental/[phone_name][.20o/.21o/.nmea] - GPSコミュニティで使用されている他のフォーマットのgnssログと同等のデータ。
- baseline_locations_[train/test].csv - 簡単な方法で生成された推定座標です。
- ground_truth.csv - 予想されるタイムスタンプでの基準となる位置。
- millisSinceGpsEpoch - GPSエポック(1980/1/6 midnight UTC)からのミリ秒単位の整数値。その値は次のようになります
- round(Raw::TimeNanos - Raw::FullBiasNanos / 1000000.0)
となります。この値は、Raw文に記述された各エポックごとに
- latDeg, lngDeg - 基準となるGNSS受信機（NovAtel SPAN）で推定されたWGS84の緯度、経度（10進法）。位置を非整数のタイムスタンプに合わせるために、必要に応じて線形補間が適用されています。
- heightAboveWgs84EllipsoidM - 基準となるGNSS受信機によって推定されたWGS84楕円体からの高さ（単位：メートル）です。
- timeSinceFirstFixSeconds - 最初の位置修正からの経過時間（秒）。
- hDop - Horizontal dilution of precision DOP（GGA文中のDOPの水平方向の希釈）で、測定値の誤差が最終的な水平方向の位置推定にどのように影響するかを表しています。
- vDop - 精密DOPの垂直方向の希釈（GSA文より）、測定値の誤差が最終的な垂直方向の位置推定にどのように影響するかを説明しています。
- speedMps - 地上での速度をメートル毎秒で表したものです。
- courseDegree - 地上での真北を基準とした時計回りのコース角度（単位：度）です。

[train/test]/[drive_id]/[phone_name]/[phone_name]_derived.csv - 生のGNSS測定値から派生したGNSS中間値で、利便性のために提供されています。

これらの派生値を使用して、補正された疑似距離（電話機から衛星までの幾何学的な距離の近似値）を次のように計算できます： correctedPrM = rawPrM + satClkBiasM - isrbM - ionoDelayM - tropoDelayM。ベースラインの位置は、補正されたPrMと衛星の位置を使用して、標準的な重み付き最小二乗（WLS）ソルバーを用いて計算されます。このソルバーには、各エポックの状態として、携帯電話の位置（x、y、z）、クロックバイアス（t）、各固有信号タイプのisrbMが設定されています。
- collectionName - "grand "の親フォルダの名前です。
- phoneName - 親フォルダの名前。
- millisSinceGpsEpoch - GPSエポック(1980/1/6 midnight UTC)からのミリ秒単位の整数。
- constellationType。GNSSコンステレーションタイプ。constellation_type_mapping.csvにマッピング文字列の値が記載されている整数値です。
- svid - 衛星ID。
- signalType - GNSS信号タイプは、コンステレーション名と周波数帯の組み合わせです。スマートフォンで測定される一般的な信号タイプは以下の通りです。GPS_L1、GPS_L5、GAL_E1、GAL_E5A、GLO_G1、BDS_B1I、BDS_B1C、BDS_B2A、QZS_J1、QZS_J5。
- receivedSvTimeInGpsNanos - チップセットが受信した信号の送信時間で、GPSエポック以降のナノ秒数で表します。ReceivedSvTimeNanosから変換すると、この派生値はすべての星座で統一された時間スケールになりますが、ReceivedSvTimeNanosはGLONASSでは一日の時間、GLONASS以外の星座では週の時間を参照します。
- [x/y/z]SatPosM - ttx = receivedSvTimeInGpsNanos - satClkBiasNanos (以下に定義)で定義される "真の信号送信時間 "の最良の推定値におけるECEF座標フレーム内の衛星位置(メートル)。これらは、衛星放送のエフェメリスを用いて計算され、真の衛星位置に対して約1mの誤差があります。
- [x/y/z]SatVelMps - 信号送信時刻(receivedSvTimeInGpsNanos)におけるECEF座標フレーム内の衛星速度(meters per second)。これらは、衛星放送のエフェメリスを用いて、このアルゴリズムで計算されます。
- satClkBiasM - 信号送信時刻(receivedSvTimeInGpsNanos)におけるハードウェア遅延を組み合わせた衛星時刻補正(メートル単位)。その時間に相当するものがsatClkBiasNanosと呼ばれています。
satClkBiasNanosは、satelliteTimeCorrectionからsatelliteHardwareDelayを差し引いた値に相当します。
IS-GPS-200H の 20.3.3.3.1 節で定義されているように、satelliteTimeCorrection は∆tsv = af0 + af1(t - toc) + af2(t - toc)2 + ∆tr から計算され、satelliteHardwareDelay は 20.3.3.3.2 節で定義されている用語です。
上記の式のパラメータは、衛星放送のエフェメリスに記載されています。
- satClkDriftMps - 信号送信時刻（receivedSvTimeInGpsNanos）における衛星クロックのドリフト（meter per second）。これは、t+0.5sとt-0.5sにおける衛星クロックのバイアスの差に相当します。
- rawPrM - メートル単位の生の疑似距離。これは、光速と、信号送信時刻(receivedSvTimeInGpsNanos)から信号到着時刻(Raw::TimeNanos - Raw::FullBiasNanos - Raw::BiasNanos)までの時間差の積である。
- rawPrUncM - メートル単位の生の疑似レンジの不確かさ。これは、光の速度とReceivedSvTimeUncertaintyNanosの積です。
- isrbM - 非GPS-L1信号からGPS-L1信号への信号間距離バイアス（ISRB）をメートル単位で表したもの。例えば、GPS L5のisrbMが1000mの場合、GPS L5の疑似レンジは、同じGPS衛星が送信するGPS L1の疑似レンジよりも1000m長いことを意味します。GPS-L1信号ではゼロです。ISRBは、GPSチップセット・レベルで導入され、重み付けされた最小二乗エンジンの状態として推定されます。
- ionoDelayM - Klobucharモデルで推定された電離層遅延（メートル単位）。
- tropoDelayM - Nigel Penna, Alan Dodson and W. Chen (2001)によるEGNOSモデルを用いて推定された対流圏遅延時間（メートル単位）。 

[train/test]/[drive_id]/[phone_name]/[phone_name]_GnssLog.txt - GnssLoggerアプリによって生成された携帯電話のログです。このノートブックでは、ログを解析する方法を説明します。各 gnss ファイルにはいくつかのサブデータセットが含まれており、それぞれの詳細は以下のとおりです。


Raw - Android API GnssMeasurementから収集された、1つのGNSS信号 (L5対応のスマートフォンでは、各衛星に1～2つの信号がある場合があります) の生のGNSS測定値。
- utcTimeMillis - UTCエポック(1970/1/1)からのミリ秒で、GnssClockから変換されたものです。
- TimeNanos - ナノ秒単位のGNSS受信機の内部ハードウェアクロック値。
- LeapSecond - クロックの時間に関連付けられている閏秒です。
- TimeUncertaintyNanos - クロックの時間の不確実性 (1シグマ) をナノ秒で表したものです。
- FullBiasNanos - GPS受信機内部のハードウェアクロックgetTimeNanos()と1980年1月6日0000Z以降の真のGPS時間との差をナノ秒単位で表したものです。
- BiasNanos - クロックのサブナノ秒のバイアスです。
- BiasUncertaintyNanos - 時計のバイアスの不確かさ（1シグマ）をナノ秒単位で表したものです。
- DriftNanosPerSecond - 時計のドリフトを1秒あたりのナノ秒で表したもの。
- DriftUncertaintyNanosPerSecond - 1秒あたりのクロックのドリフトの不確かさ(1シグマ)をナノ秒で表したものです。
- HardwareClockDiscontinuityCount - ハードウェアクロックの不連続性のカウント。
- Svid - 衛星IDです。詳細はこちらをご覧ください。
- TimeOffsetNanos - 測定が行われた際のタイムオフセットをナノ秒単位で表したもの。
- State - 衛星の同期状態を示す整数。整数の各ビットは、測定の特定の状態情報を表す。ビットと状態の間のマッピングについては、metadata/raw_state_bit_map.jsonファイルを参照してください。
- ReceivedSvTimeNanos - 測定時刻における、受信したGNSS衛星の時刻をナノ秒単位で表したものです。
- ReceivedSvTimeUncertaintyNanos - 受信したGNSS時間の誤差推定値（1シグマ）をナノ秒単位で表したものです。
- Cn0DbHz - 搬送波対雑音比をdB-Hzで表したもの。
- PseudorangeRateMetersPerSecond - タイムスタンプでの疑似レンジレートをm/sで表したもの。
- PseudorangeRateUncertaintyMetersPerSecond - 擬似レンジのレートの不確かさ（1シグマ）をm/sで表したもの。
- AccumulatedDeltaRangeState - これは、「Accumulated Delta Range」測定の状態を示します。整数の各ビットは、測定の状態を表しています。ビットと状態の間のマッピングについては、metadata/accumulated_delta_range_state_bit_map.jsonファイルを参照してください。
- AccumulatedDeltaRangeMeters - 最後のチャンネルリセット以降の累積デルタレンジ、単位はメートルです。
- AccumulatedDeltaRangeUncertaintyMeters - 累積されたデルタレンジの不確かさ(1シグマ)をメートル単位で表したものです。
- CarrierFrequencyHz - トラッキングされた信号のキャリア周波数です。
- CarrierCycles - 衛星と受信機の間の完全なキャリアサイクルの数です。これらのデータセットではNullです。
- CarrierPhase - 受信機で検出されたRF位相です。これらのデータセットではNullです。
- CarrierPhaseUncertainty - 搬送波位相の不確かさ(1シグマ)です。これらのデータセットではNullです。
- MultipathIndicator - イベントの「マルチパス」状態を示す値です。
- SnrInDb - (相関と統合後の)信号対雑音比(SNR)をdBで表したもの。
- ConstellationType - GNSSコンステレーションタイプ。整数値で、文字列値へのマッピングはconstellation_type_mapping.csvファイルで提供されています。
- AgcDb - 自動利得制御レベル（単位：dB）です。
- BasebandCn0DbHz - ベースバンドのCarrier-to-Noise密度をdB-Hzで表した値。Android 11でのみ利用可能です。
- FullInterSignalBiasNanos - GNSS測定の信号間バイアスをナノ秒単位で表したもので、サブナノ秒の精度を持ちます。2021年にPixel 5 logsでのみ利用可能。Android 11でのみ利用可能です。
- FullInterSignalBiasUncertaintyNanos - GNSS測定の信号間バイアスの不確かさ（1シグマ）をナノ秒単位で、サブナノ秒の精度で表したもの。Android 11でのみ利用可能です。
- SatelliteInterSignalBiasNanos - GNSS測定の衛星信号間バイアスをナノ秒単位で表したもので、サブナノ秒の精度で表示されます。Android 11でのみ利用可能です。
- SatelliteInterSignalBiasUncertaintyNanos - GNSS測定の衛星信号間バイアスの不確かさ（1シグマ）をナノ秒単位で表したもので、サブナノ秒の精度を持ちます。Android 11でのみ利用可能です。
- CodeType - GNSS測定のコードタイプ。最近のログでのみ利用可能です。
- ChipsetElapsedRealtimeNanos - システム起動時からのこのクロックの経過したリアルタイムをナノ秒単位で表したものです。最近のログでのみ利用可能です。

Status - Android API GnssStatusから収集されたGNSS信号のステータスです。
- UnixTimeMillis - UTCエポック(1970/1/1)からのミリ秒で、GPSプロバイダが最後に変更した場所から報告されます。
- SignalCount - 衛星リスト内の衛星の合計数。
- SignalIndex - 現在の信号のインデックスです。
- ConstellationType。指定されたインデックスの衛星のコンステレーションタイプ。
- Svid：衛星のIDです。
- CarrierFrequencyHz Cn0DbHz：追跡している信号の搬送波周波数
- Cn0DbHz。Cn0DbHz：指定されたインデックスの衛星のアンテナにおけるcarrier-to-noise densityをdB-Hzで表したもの。
- AzimuthDegrees 指定されたインデックスにおける衛星の方位を示す。
- ElevationDegrees：指定インデックスにおける衛星の仰角 ElevationDegrees：指定されたインデックスにおける衛星の仰角。
- UsedInFix: 指定されたインデックスの衛星が、最新の位置修正の計算に使用されたかどうか。
- HasAlmanacData。指定されたインデックスの衛星がアルマナックデータを持っているかどうか。
- HasEphemerisData: 指定されたインデックスの衛星がエフェメリスデータを持っているかどうか。
- BasebandCn0DbHz。BasebandCn0DbHz：指定されたインデックスの衛星のベースバンド搬送波ノイズ密度をdB-Hzで表したもの。

UncalAccel - Android APIのSensor#TYPE_ACCELEROMETER_UNCALIBRATEDから収集された、校正されていない加速度計からの読み取り値。
- utcTimeMillis - 以下のelapsedRealtimeNanosと、最近のNTP (Network Time Protocol)同期後のUTCでの推定デバイス起動時間の合計です。
- elapsedRealtimeNanos。イベントが発生した時間をナノ秒単位で表したもの。
- UncalAccel[X/Y/Z]Mps2 - [x/y/z]_uncalib（バイアス補正なし）です。
- Bias[X/Y/Z]Mps2 - 推定された[x/y/z]_biasです。以前の日付で収集されたデータセットではNullとなります。

UncalGyro - Android APIのSensor#TYPE_GYROSCOPE_UNCALIBRATEDから収集された、キャリブレーションされていないジャイロスコープからの読み取り値
- utcTimeMillis - 以下のelapsedRealtimeNanosと、最近のNTP（Network Time Protocol）同期後のUTCでの推定デバイス起動時間の合計です。
- elapsedRealtimeNanos。イベントが発生した時間をナノ秒単位で表したもの。
- UncalGyro[X/Y/Z]RadPerSec - [X/Y/Z]軸周りの角速度（ドリフト補正なし）をrad/sで表したものです。
- Drift[X/Y/Z]RadPerSec - [X/Y/Z]軸周りの推定ドリフト量をrad/sで表したものです。以前の日付で収集されたデータセットでは無効です。

UncalMag - Android APIのSensor#STRING_TYPE_MAGNETIC_FIELD_UNCALIBRATEDから収集された、校正されていない磁力計からの読み取り値です。
- utcTimeMillis - 以下のelapsedRealtimeNanosと、最近のNTP（Network Time Protocol）同期後のUTCでの推定デバイス起動時間の合計。
- elapsedRealtimeNanos。イベントが発生した時間をナノ秒単位で表したもの。
- UncalMag[X/Y/Z]MicroT - [x/y/z]_uncalib（バイアス補正なし）。
- Bias[X/Y/Z]MicroT - 推定された[x/y/z]_biasです。以前の日付で収集されたデータセットではNullです。

OrientationDeg - 各行は、Android APIのSensorManager#getOrientationから収集された、デバイスの推定方位を表します。このメッセージは、2021年3月以降に収集されたログでのみ利用可能です。
- utcTimeMillis - 以下のelapsedRealtimeNanosと、最近のNTP（Network Time Protocol）同期後のUTCでの推定デバイス起動時間の合計です。
- elapsedRealtimeNanos - イベントが発生した時のナノ秒単位の時間です。
- yawDeg - 画面がポートレートモードの場合、この値はアジマス度（0°～360°のモジュラス）に相当します。画面がランドスケープモードの場合は、画面の回転角度（90°または270°）と方位角の和（0°〜360°へのモジュラス）に相当します。方位角とは、-z軸を中心とした回転角度のことです。この値は、デバイスのy軸と磁北極の間の角度を表します。
- rollDeg - Roll、y軸を中心とした回転の角度を指す。この値は、デバイスの画面に垂直な平面と、地面に垂直な平面との間の角度を表します。
- pitchDeg - ピッチ、x軸を中心とした回転の角度。この値は、デバイスの画面に平行な平面と、地面に平行な平面との間の角度を表します。



# log
### 20210530
- 最後まで走り切ろう。titanicの壁を越えたい
- join(ころんびあさんのツイートに初心者でもいけるって書いてあったから)
- data download
- ピン留めされてるディスカッション読んだ
  - [外部データセット](https://www.kaggle.com/c/google-smartphone-decimeter-challenge/discussion/238579)
  - [ベースラインの位置推定の方法](https://www.kaggle.com/c/google-smartphone-decimeter-challenge/discussion/238583)(いつか役に立ちそう)
    - ベースライン推定値の平滑化。これには基本的にドメインの知識は必要ありません。
    - 加速度計のような他の携帯電話機器からの読み取り値の統合。
    - Derived.csvファイルを使った衛星の三角測量。
    - gnssの生ログから直接三角測量を行う。これは動作させるのに時間がかかるかもしれませんが、エラー修正の機会が増えるはずです。
    - その地域の基地局からの衛星測定値のようなコントロールのための外部データを組み込む。
  - の段階に分かれているらしい
  - 三角測量
    - <img width="663" alt="スクリーンショット 2021-05-30 17 49 02" src="https://user-images.githubusercontent.com/71954051/120098164-549c6b80-c16f-11eb-9906-829d982f1fe1.png">
    - [簡単な説明](https://www.kurabo.co.jp/el/room/3d/page1.html)
  - GNSS : GNSS(global navigation satellite system)は、衛星測位システムの総称で、複数の測位衛星から時刻情報つきの信号を受信し、地上での現在位置を計測するシステムです。 GPS(global positioning system)はアメリカ合衆国が開発したシステムであり、GNSSの1つです。

### 20210531
- data理解
  - [このディスカッション](https://www.kaggle.com/c/google-smartphone-decimeter-challenge/discussion/241039)読んだ。とりあえず
最初はcsvファイルだけ。他のファイルはGNSSの説明書いてあるから、コンペ中盤以降読む。
  - [Let's visualize dataset to understand! 🚙三](https://www.kaggle.com/nayuts/let-s-visualize-dataset-to-understand)読んだけど、importでエラー出た。localはあかん。kaggle kernelならできるから、EDAは基本ここやな
- [public1位のnotebook](https://www.kaggle.com/dehokanta/baseline-post-processing-by-outlier-correction)でカルマンフィルタが使われている
  - [カルマンフィルタ](https://qiita.com/IshitaTakeshi/items/740ac7e9b549eee4cc04)

### 20210601
- [Let's visualize dataset to understand! 🚙三](https://www.kaggle.com/nayuts/let-s-visualize-dataset-to-understand)
  - 車が動くGIF
    - 同じ道を通ることがある
    - 曲がり道で一時停止している(信号、曲がり角は減速するとか)
  - rollDegs面白い。スマホの向きがわかれば、次に行く方向わかる。2021/5月以降しか記録されていない。
- nb01: ベースラインそのまま出した。lb7.187、232位。ここから頑張ろう。
- kalman filter と keras良さげ？

### 20210602
- nb02
  - cv: 3.391, lb: 6.217
  - [Demonstration of the Kalman filter](https://www.kaggle.com/emaerthin/demonstration-of-the-kalman-filter)そのまま
  - カルマンフィルターの引数が全くわからん

### 20210603
- インターンと課題やってたという言い訳。朝9時過ぎに起きたのが悪い

### 20210604(1時間)
- nb003
  - [ベイズ最適化による、カルマンフィルターのパラメータチューニング](https://www.kaggle.com/tqa236/kalman-filter-hyperparameter-search-with-bo)
- EKF(拡張カルマンフィルター)使えるかも？[Demonstration of the Kalman filter](https://www.kaggle.com/emaerthin/demonstration-of-the-kalman-filter)のコメントに書いてあった
- [WGS84座標からxyz直交座標への変換](https://www.kaggle.com/c/google-smartphone-decimeter-challenge/discussion/241453)
- WGS84は、米国が構築・維持している世界測地系であり、GPSの軌道情報で使われているほか、GPSによるナビゲーションの位置表示の基準として使われています。

- nb005[Qiita](https://qiita.com/r-fuji/items/99ca549b963cedc106ab)
    - 二点間の距離の算出 : Vincenty法の方がhaversine法よりも精度良いか？[ソース](https://qiita.com/7of9/items/4ea7356a8806eaa08d5e#comment-8b584fc3d15249477719)
- 車が曲がり角で遅くなる : 遅くなった時は誤差小さく、速く動いている時は誤差を大きめにとると良いかも

- nnb005 : [phone mean prediction](https://www.kaggle.com/t88take/gsdc-phones-mean-prediction)

### 20210605
- vencityあんまよくないらしい。geopyでKarney法。[ソース](https://h-memo.com/python-geopy-distance/)
- nb007
  - 今までやってたの、get_train_scoreだった。まじで意味ない。何がしたいかちゃんと考えよう

- nb??? reject outlier[Baseline post-processing by outlier correction](https://www.kaggle.com/dehokanta/baseline-post-processing-by-outlier-correction)
  - base_trainとground_truthの距離の差が大きいものがある。
  - test野中にはgtが入っていないので、lat,lonを前後にshiftすることで、次の時点との距離を算出。大きいものはあるが、これは外れ値か直線上でスピードを出したのだと推測できるので、前後の位置の1/2を現在地と修正する。大きいか否かは、95%信頼区間である、std*2で判断
- nb008
  - nb004では外れ値を削除してた。nb008では前後の距離の1/2をとったけど、スコア改善にはならなかった。cvとlbの相関は取れてそう。
- 位置のdiff出したい。外れ値と、直線上の動き判断できそう

### 20210606
- 用事が結構入ってた。すみません。

### 20210607
- [何かの最適化](https://www.kaggle.com/foreveryoung/least-squares-solution-from-gnss-derived-data)
  - 衛星とのレシーバーの距離誤差を最小化したい。
    - `correctedPrM = rawPrM + satClkBiasM - isrbM - ionoDelayM - tropoDelayM`
    - 擬似範囲 = 実際の距離 + ユーザーのクロックバイアス + 実際のユーザーの位置

### 20210608
- [nb010](https://www.kaggle.com/foreveryoung/least-squares-solution-from-gnss-derived-data)
  - 擬似距離 = (受信機でその信号が衛星から受信機に届くまでの時間遅れ)×(伝播速さ(＝光速))

### 20210609
- 0からデータとか評価を見直した
  - nb011
    - [Baseline from host data](https://www.kaggle.com/jpmiller/baseline-from-host-data)わかりやすい。
    - 自分たちはtest_baseを予測する
    - 評価はtest
  - nb012
    - [simdkalman公式](https://simdkalman.readthedocs.io/en/latest/)
    - 次回kalmanフィルターの使い方から

### 20210610
- カルマンフィルターの使い方
  - base_train, gtでをsmoothし、そのkfでノイズが混じったtestの位置を平滑化
  - 特徴量からターゲットを予測するわけではない
  - 時点1から時点tまでの観測データy1:ty1:tを元に、ある時点の状態xt′xt′を推定します。つまり、p(xt′|y1:t)p(xt′|y1:t)を求めます。より正確には、観測データが手元にあるときの時点tの状態xtxt確率分布p(xt′|y1:t)p(xt′|y1:t)を求め、その平均や分散を求めます。[参考](https://qiita.com/hanon/items/7f03621414c59f06d7ca)
- nb004
  - 閾値は50が良いみたい
- [ころんびあさんのディスカッション](https://www.kaggle.com/c/google-smartphone-decimeter-challenge/discussion/244752)
  - T88さん: predicts speed-related metrics for post-processing.
This method has worked well
  - post-processingがかなり効くみたい
  - 次回post-processingのcode読む！！

### 20210611
- [ころんびあさんのディスカッション](https://www.kaggle.com/c/google-smartphone-decimeter-challenge/discussion/244752)
  - みんなベースラインのデータ使っているみたい。IMUデータ使ったらベースライン超えられそうだけど結合させるのめんどくさいし、うまくいかないみたい。いつか使えそう。code待ち
  - MLではない。post processing使う

### 20210612
- nb015
  - nb014ではphone_mean_predをした後にremove_deviceしている
  - 今回は逆にしてみた
  - lb悪化
- nb016
  - subファイルをpandasで読み込んでからremove_deviceした
  - lb悪化: 5.639 → 5.596
- [GSDC blend](https://www.kaggle.com/rtombs/gsdc-blend)
  - 公開notebookの中で一番強い。lb: 5.558

### 20210613
- nb017
  - [position shift](https://www.kaggle.com/wrrosa/gsdc-position-shift)コピー
  - ```
    for fi in ['x','y','z']:
        d[[fi+'new']] = d[fi+'p'] + d[fi+'diff']*(1-a/d['dist'])
    ```
  - シフトさせる次の地点は、現在地と、次の地点(diff)との差を距離でスケーリングしたものを足して算出される
  - スケーリング係数をoptunaで最適化する
  - lb: 5.531
  - post processingの順番
    1. Reject outlier
    2. Kalman filter
    3. Phone mean prediction
    4. remove device
    5. Position shift
- [ころんびあさんディスカッション](https://www.kaggle.com/c/google-smartphone-decimeter-challenge/discussion/245221)
  - 今回のコンペはpost processingの順番によってスコアが変わるらしい
  - 21345の順番だとスコア下がるらしい
- nb018
  - dehokantaさんのやつとphone mean predictionのreject outlierどっちが効くのか検証
  - cv: 4.58290973646303
  - nb005のphone mean prediction内のreject outlierではcv: 4.551122113134492だった
  - 結論: phone mean predictionの方が精度よくなる
- nb019
  - [GSDC EDA : Error when stopping](https://www.kaggle.com/t88take/gsdc-eda-error-when-stopping)
  - 止まった時の誤差が気になる。gtに比べてかなり散らばっている。他は直線、曲がり角かかわらず割と良い線行ってる
- 走行した地理条件によって、精度変わるか？
  - ただ、同じコレクションでも精度違った
  - コレクションを横断して、phoneの精度と、地理条件を考えるべき

### 20210614
- 授業と課題

### 20210615
- plotlyの導入

### 20210616
- nb019
  - 減速した時の誤差が大きい
  - speed=0多い  
  <img width="363" alt="スクリーンショット 2021-06-16 11 42 04" src="https://user-images.githubusercontent.com/71954051/122149956-e0183b00-ce97-11eb-9967-75f19eb50fee.png">
  - speedMpsと、次の地点への距離(move dist)のグラフが似ている。座標を元に計算されている？
    <img width="571" alt="スクリーンショット 2021-06-16 11 41 13" src="https://user-images.githubusercontent.com/71954051/122149892-c1b23f80-ce97-11eb-842d-a1ec6620911c.png">  
    - gtのspeedMpsとmove dist同じなの当たり前ではないのか？
    - haighwayはやっぱり精度良い
- フィルタリングしたtrainのcsvファイルも作った
  - nb005, nb016, nb017
- nb020
  - filteredファイルに対して、nb019と同じEDAをした
  - しっかりとフィルタリングされていることがわかるから、今後はこれをベースにして案出していく
- nb017-2
  - position shift2回目やったら効く気がした
  - lb悪化: 5.564
- [Taroさんdiscussion](https://www.kaggle.com/c/google-smartphone-decimeter-challenge/discussion/245160)
  - high way、tree-lined street、downtownではGNSSも精度が違う。これごとにrunを変える必要がある
### 20210617
- 橋の下だと精度悪くなる？
<img width="902" alt="スクリーンショット 2021-06-17 8 54 23" src="https://user-images.githubusercontent.com/71954051/122309689-b15a9d00-cf49-11eb-986f-96dc93bd3610.png">
- nb019とnb020もっと見る

### 20210618
- 課題進めた

### 20210620
- 課題進めた

### 20210621
- 課題進めた

### 20210622
- 初期点ずれがち
- gtとの誤差(dist_err)が大きいか否かを機械学習で求めたら良さそう
- とにかく、dist_errが大きいものを検知したい

- nb021
- 外れ値をとるデータの特徴を分析
  - gtとの距離が大きいデータ, それを除去したデータ
  - ある程度移動距離小さかったら、除去しても良いかも。

<img width="157" alt="スクリーンショット 2021-06-22 11 00 55" src="https://user-images.githubusercontent.com/71954051/122851053-4a722500-d349-11eb-898a-b8bc16c9e1cb.png">

<img width="184" alt="スクリーンショット 2021-06-22 11 01 11" src="https://user-images.githubusercontent.com/71954051/122851046-480fcb00-d349-11eb-9c81-433010d53c12.png">

- 外れ値除去してもkalman filterがいい感じにしてくれる
- nb023
  - nb005では、前後両方50m以上で外れ値判定
  - describeで分析したら、前後両方だけだと漏れているデータがあった
    - 片方は20m、もう片方は8000mなど
  - ここではどちらかが50m以上で外れ値判定した
  - lb改善: 5.531→5.477 嬉しいーーー
- nb023_1
  - 外れ値除去の閾値を50→43にしたらスコア上がった
  - 5.477→5.394

### 20210623
- nb024
  - 今までのフローを一つのnotebookにまとめた
  - trainは終了
  - 次回、testデータもやる

### 20210624-20210628
- 課題、テスト勉強

### 20210629
- nb024
  - フロー整理
- nb025
  - train, submissionでわかりやすいようにコード整理
- nb020

<img width="44" alt="スクリーンショット 2021-06-29 14 17 26" src="https://user-images.githubusercontent.com/71954051/123741292-bd990f80-d8e4-11eb-8f6a-8315aed57d9f.png">
<img width="104" alt="スクリーンショット 2021-06-29 14 19 11" src="https://user-images.githubusercontent.com/71954051/123741410-fa650680-d8e4-11eb-9228-0863d9cb6743.png">

  - 第三四分位数+1.5×四分位範囲を超えたデータを外れ値として除去する
    - 今回は第三四分位数が2.aであるため、平均が最大値の方へ引っ張られてしまう。そのため、正規分布を仮定して平均値から標準偏差の3倍より大きいものを削除する方法は使えない

  <img width="427" alt="スクリーンショット 2021-06-29 14 15 28" src="https://user-images.githubusercontent.com/71954051/123741160-875b9000-d8e4-11eb-940e-a849be0d4a5b.png">

  - 外れ値(dist_err>thr)の特徴
    - hDopが高い(gtにしか含まれていない)
    - speedMps明らかに小さい
      - スピードが遅い時に多い

      <img width="120" alt="スクリーンショット 2021-06-29 14 51 49" src="https://user-images.githubusercontent.com/71954051/123744306-8a0cb400-d8e9-11eb-95cb-ea77ab13180f.png">

      - outlierの75%が7.67, roの25%が8.51だし、これは落として良いかも。roの8.51以下の数によるけど
    - dist_prevとdist_nextが210ある。これは落とすべき。
      - 最初に外れ値は落としたけど、それ以降の処理で追加されてしまった
    - collectionごとにremove device変えるの良いかも
    - もしくは機械学習
      - nb027
      - phoneは落としたけど、tree,downtownごとにラベルづけしても良いかも
      - phoneNameだけは残す
      - lightgbmは組めた
      - 欠損値をそう補完するかを次回考える
      - adversal validationが0.5にならん。。。。。。
- nb028
  - [baselineを再計算](https://www.kaggle.com/shu421/gsdc-reproducing-baseline-wls-on-one-measurement/)

- sample_subとbase_testのtimestamp全く同じだ笑 前検証した時なんで間違えたんだろう
- trainのderive,gt,base_trainで共通するtimestampある
- deriveは一つのtimestampだけ。21行くらい
- testでは共通するtimestampは限られている
- deriveにあるtimestampはbaselineの計算できるけど、ないやつはどうするんだろ

### 20210630
- サボってました、、、明日の午前やる

### 20210701
- deriveにあるtimestampのbaseline補間だけやるか、そこからさらにカルマンフィルタでデータの補間するか
- 補間するには、base_train,train_derived、base_test,test_derivedに共通するtimestampが必要
  - base_train,train_derived
    - base_trainに合ってtrain_derivedにないtimestamp = 53442
    - base_train = 131342
    - 40.7%欠損
  - base_test,test_derived
    - base_testに合ってtest_derivedにないtimestamp = 40943
    - base_test = 91486
    - 44%欠損
  1. 統合されたtrain_derivedを使ってbase_trainの距離推定
  2. ground_truthとの距離確認
  3. 改善されていたらtest_derivedを使ってtest_derivedの距離推定

### 20210702-0703
- groupbyと戦っていた

### 20210704
- nb029
  - nb028をfor文で回した
  - 17hくらいかかりそうだから、applyとgroupbyの勉強する
- nb030
  - nb029のテスト用
- nb031
  - applyとgroupbyの勉強
  - 関数の出力がよくなかった。タブルで出力していたところをリストにしたらapplyすることができた
  - applyにしても計算時間は長くなるので、pandarallelを使って並列処理した
- nb032
  - [Predict Next Point with the IMU data](https://www.kaggle.com/alvinai9603/predict-next-point-with-the-imu-data)
  - IMU使った、次点の予測
- nb033
  - nb032を今までのパイプラインに適用
  - 強すぎる.... lb5.394→5.297
- nb027
  - lightgbmで外れ値検出
  - [u++さんの記事](https://upura.hatenablog.com/entry/2019/10/27/211137)によると、過去コンペではaucを0.98から0.7程度に下げた例がある。0.7は結果論


### 20210705
- nb030
  - base_testの位置再計算した
- sub_nb033_1
  - nb033を使って予測
  - lb: 294.634
  - サンプルが少ないデータについても位置の計算をしていたからだと考えられる
- sub_nb033_2
  - sub_nb033_1を受けて、サンプル数が21以上のmillisSinceGpsEpochについて、位置の計算を行った
  - lb: 85.215
  - なんでだろう...
  - 外れ値の閾値を43m→50mにしたらむしろスコア上がった(85.199)。多分誤差
- nb027
  - millisSinceGpsEpochを落としたらauc=0.89
  - latDeg, lngDegも落とす
  - 落としても0.89
  - 前後の位置(latDeg_prevとか)が含まれているからだろうな
  - そういう系全部落とすと、latDeg_prev系とdist_prevとdist_nextだけになる(6つ)
  - f1 score: 0.2264
  - 流石に特徴量少ないから、snap to gridの特徴量も加えてみる
- nb034
  - [snap to grid](https://www.kaggle.com/kuto0633/road-detection-and-creating-grid-points)コピペ
- 次回、snap to gridの特徴量をnb027に追加する

### 20210706
- nb035
  - nb034を全てのデータに適用

- アイデア: tree, downtown, highwayごとにphoneの重み変えてみるとか？
  - それぞれのphoneの得意分野ありそう

- nb025
  - ro, kf, pm, rm, ps : 3.847222643885028
  - ro, kf, rm, pm, ps :  4.095626241286455
  - ro, rm, kf, pm, ps :  4.086193360584251
  - ro, rm, pm, kf, ps :  4.088935286476016
  - ro, kf, pm, rm, ps, kf :  3.888487843396948

### 20210708
- 面接とかバイト(言い訳です。1コミットでも良いからすればよかった)

### 20210708
- nb037
  - nb032の実験用
  - nb032ではSJCの予測している。
  - SJCは202103以降に収集されているので、GNSSのStatusが使える
  - 202103以前も、[Estimating the direction with a magnetic sensor](https://www.kaggle.com/museas/estimating-the-direction-with-a-magnetic-sensor)使って方向の予測すればできるのでは
  - とりあえず、202103以降のデータだけIMUで予測する
- sub_nb033_3
  - MTVを学習させてSJCを予測した
  - lb: 705.836
  - 同じcollectionを学習しなければならない
  - SVL: bl, cv: 3.7596290065930966, 11.88683932055823
  - SJCはdowntownだから最も効果出る
  - '2021-04-29-US-SJC-3'以外のtestのSJC、phoneもやる
- nb037, sub_nb033_3, sub_nb037
  - 2021-04-29-US-SJC-3_Pixel4をIMUから予測
  - lb改善: 5.297→5.273
  - SJC全部やったら相当上がるなこれ

- nb038
  - [Data divided into 3 areas(downtown,highway,tree)](https://www.kaggle.com/tyonemoto/data-divided-into-3-areas-downtown-highway-tree)

- 最後に外れ値検出してinterporateで補完する

- sub_nb033_4
  - position shiftを消してみた
  - cvだと、position shiftない方が良かった。cvがあっているかの確認
  - lb 5.364
  - 多分baselineの作り方がおかしい

- nb033
  - position shift入れるとcv悪化、lb改善。

- 賢くない実装をしました
- nb037_1
  - nb037 + 2021-04-22-US-SJC-2_SamsungS20Ultra
- nb037_2
  - nb037_1 + 2021-04-02-US-SJC-1_Pixel4
  - これ地雷
- nb037_3
  - nb037_2 + 2021-04-02-US-SJC-1_Pixel5
  - これ地雷
- nb037_4
  - nb037_3 + 2021-04-22-US-SJC-2_SamusungS20Ultra

- sub_nb003_5
  - nb037_4のサブ
  - 何かミスってるみたい(cvがおかしい)
  - 次直す

### 20210709
- 前の全部通したやつスコアおかしいから、全部の段階でsubしてみる
- subnb033_6
  - nb037_1
  - lb: 5.311
- sub_nb033_7
  - nb037_2
  - lb: 226.462
- sub_nb033_8
  - nb037
  - lb: 5.311
- sub_nb033_9
  - sub_nb037(base_train = baseline)
  - lb: 5.273

- nb037_5
  - sub_nb037 + 2021-04-29-US-SJC-3_SamsungS20Ultra

- sub_nb033_10
  - nb033_5
  - lb: 5.320
- なんでPixel4、samusung個別ならスコア上がるのに一緒だとちょっと悪くなるんだ〜ーー

- sub_nb033_11
  - nb037_3
  - baseline + 2021-04-02-US-SJC-1_Pixel5のIMU
  - lb: 227.030

- sub_nb033_12
  - nb037_4
  - baseline + 2021-04-22-US-SJC-2_SamusungS20Ultra
  - lb: 5.394

- nb036
  - EDA
  - dist_errをdist_prev,dist_nextから探索したい
  - thr = 75%+1.5×IQRだとあかん
  - outlierのdist_prev,nextのmaxと、reject_outlierのdist_prev,nextのminを一致させる閾値を見つけた
  - outlier(閾値よりdist_errが大きいやつ)の散布図
  - 右の方のやつは落とすとして、dist_prev==0の外れ値が多すぎる
  <img width="456" alt="スクリーンショット 2021-07-10 10 59 04" src="https://user-images.githubusercontent.com/71954051/125148529-e5526800-e16d-11eb-9eaf-b023fb928196.png">

  - (紫)2021-04-22-US-SJC-1_Pixel4, SamusungS20Ultraかなりずれてる
  - (ピンク)2021-04-29-US-SJC-2_Pixel4, SamusungS20Ultraに統合させても良いかも
  - (赤)2021-04-28-US-SJC-1_Pixel4, SamusungS20Ultraは実験して決める

  ### 20210711
 - nb036
   - SJCのmean_scoreと、それぞれのスコアのmean見比べる
   - 2021-04-22-US-SJC-1_Pixel4: 15.40513722208054
   - 2021-04-22-US-SJC-1_SamsungS20Ultra: 15.12562785078837
   - 2021-04-28-US-SJC-1_Pixel4: 14.610152209310147
   - 2021-04-28-US-SJC-1_SamsungS20Ultra: 14.603669303595279
   - 2021-04-29-US-SJC-2_Pixel4: 12.538784411167697
   - 2021-04-29-US-SJC-2_SamsungS20Ultra: 12.377565493740669

   - 生データ通り、04-29の精度が良い
   - SamsungS20Ultraの方が、Pixel4より精度良い

- マルチパスを発生させているGPSを探して除去するのは？

- nb036
  - dist_prev,nextが0.4以下のもの落としたら203.93952681071983になった。

### 20210713
- phoneごとに移動距離短いやつ落として線形補間したら精度上がった。nearestも試したい

### 20210713
- 今日明日はテスト勉強する

### 20210714
- nb039: remove low speed
  - 補間方法をlinearからnearestにしたらスコア改善した
  - before remove lowSpeed 3.8472226439245767
  - after remove lowSpeed 3.022374370494192
  - 補間できていなかった、、、
  - ただ、dist_prevが小さいものを削除しただけでこんなに上がったんだから、ここの処理が本当に重要

  - スタート時は次の位置、ゴール時は前の位置を使って補間: 3.831576198421017
  - linear補間: 3.8006633431449037
  - nearest→linear補間: 3.8371075152805587
  
- sub_nb033_13
  - nb037→nb033:  reject outlier + kalmanfilter:  4.498849117070059  
                  phone mean pred :  4.034688363261449  
                  position shift:  3.847222643885028  
                  remove low speed:  3.8006633431440395  
                  lb: 5.177  

- nb040
  - EDA

- sub_nb033_14
  - nb037→nb037_5→nb033
  - lb: 5.223 やっぱりだめかー

### 20210715
- nb033(filtered_nb037_nb033)
  - position shift パラメータチューニング
    - a=0.34811019724370135: 3.836874981966638
  - remove low speed パラメータチューニング
    - 0.47286807645914747: 3.787943543589775
- sub_nb033_13_1
  - sub_nb033_13をパラメータチューニング
  
- nb041
- sub_nb041 rmlsだけパラメータチューニング
  - rmls :0.6939300630849313
  - cv: 3.803592591885454
  - lb: 5.148

### 20210716
- nb042: EDA
- nb043
  - nb034(snap to grid)のコピー
  - 色々試す
  - trainのコレクション使って、元の位置との距離算出。
  - 閾値よりも小さいものを採用してみる

- nb044
  - nb037実験
    - マルチパスの影響が大きい衛星削除も考えたけど、snap to gridの方が効きそう

### 20210718
- nb045
  - [Adaptive_gauss+phone_mean](https://www.kaggle.com/bpetrb/adaptive-gauss-phone-mean)コピー
- nb046
  - phone meanをnb041に加えた
  - psmの位置
  - cv(ro, kf, pm, rm, ps rmls, psm): 3.4515872201087956
  - cv(ro, kf, pm, rm, ps, psm, rmls): 3.4477104031444266
    - psm→rmlsが効果ある
    - meanとってから誤差修正してる
    - rmls→psmだと、誤差修正してからmean。精度悪化しとる
  - cv(ro, kf, pm, rm, psm, ps, rmls): 3.431622519264048
    - psの前でデータ綺麗にしとくと良さげ
  - cv(ro, kf, pm, psm, rm, ps, rmls): 3.4768515049181348
    - phones meanした後でremove deviceしても意味ない

  - rmlsの位置
  - cv(ro, kf, pm, rmls, rm, psm, ps): 3.466
  - cv(ro, kf, pm, rm, rmls, psm, ps): 3.457784659817073
  - cv(ro, kf, pm, rm, psm, rmls, ps): 3.4184543506885996  ### 採用
    > psの前でデータ綺麗にしとくと良さげ
    - 本当にこれだった
  - cv(ro, kf, pm, rm, psm, ps, rmls): 3.431622519264048

- sub_nb046_1
  - cv(ro, kf, pm, rm, ps rmls, psm): 3.4515872201087956
  - lb: 5.046

- sub_nb046
  - cv(ro, kf, pm, rm, psm, rmls, ps): 3.4184543506885996
  - lb: 4.997
  - 4代嬉しいな！！！

### 20210719
- nb048: EDA

### 20210720
- 休息の日

### 20210721-20210724
- AI Questしてた

### 20210725
- nb049
  - snap to grid
  - thr = 10
    - original: 19.81370527195164
    - snap: 19.75985925420199
  - thr = 15
    - original: 19.81370527195164
    - snap: 19.395721673600463
  - thr = 20
    - 19.81370527195164
    - 18.663785791885832
  - thr = 30
     - 19.81370527195164
     - 17.803037634589707
  - thr = 40
    - 19.81370527195164
    - 17.90673739332115
  - thr = 50
    - 19.81370527195164
    - 18.202081162835775
  - thr = 100
    - 19.81370527195164
    - 25.60693591784196

- phoneごとに閾値決めた方が良いよなぁ
  - tree, downtown, highwayごとに散らばりかた違う
  - snap to gridは道路の中心を検知するから、道路が狭い道の方が誤差少なそう
  - 実験してみたけど、phoneごとに閾値かなり違う

- nb052
  - post processing後のデータにはsnap to grid効かないから、先にやってみる

### 20210726
- max_thr, min_thr
- SJC[1]
- 15, 10
14.898988862403762
15.175240574356103
- 18, 15
14.898988862403762
14.80115330501831
- 23, 15
14.898988862403762
14.89413637266957
- 20, 18
14.898988862403762
14.889638497163581
- 20, 15
14.898988862403762
14.768477518093087
- 19, 16
14.898988862403762
14.701296923190238
- 19, 17
14.898988862403762
14.630814483226999
- 18, 16
14.898988862403762
14.790012455848156

- SJC[0]
- 20, 15
15.734804800549322
15.018815099336818
- 19, 16
15.734804800549322
15.53062439308363
- 19, 17
15.734804800549322
15.602139032791788
- 18, 16
15.734804800549322
15.619360015595147

- SJC[2]
- 20, 15
12.501141483952647
12.53973699012966
- 20,16
12.501141483952647
12.503505077849248
- 20, 17
12.501141483952647
12.518665251444368
- 19, 16
12.501141483952647
12.483315227175172
- 19, 17
12.501141483952647
12.502238757122369
- 18, 16
12.501141483952647
12.481389898929125

- (19,16),(18,16)はSJC全て改善
- (19,16)=3.1、(18,16)=2.4程度改善
- 19,17=3.9だからこっち採用

### 20210728
- nb054
  - snap to gridをground truthでやる
    - generate point
    - search closest point
    - apply grid point

### 20210729
- low Speedをsnap to girdするとだめ
- ここだけ処理かえる

# 20210730
- sub_n055
- SJCをsnap to grid
- trainのmax_thr, min_thr = 50, 0
- test, min_thr = 30, 10 タイポしてる
  - cv: 3.142981308233182
  - lb: 4.850

# 20210801
- train: nb037, test: nb037_5でやってるのに気づいた
- subとかして検証したけど、このままで良い
- nb059
  - train_roの方がtrain_ro_kfより精度高かったからこっちでpost processingしたけど、だめだった
  - カルマンフィルターは精度上げるし、データをいい感じに並べてくれる

### 20210803
- SJC th=21
- not SJC th=45

- nb062: a car is moving or not
  - 動いていないところ移動平均とっても良いかも

### 20210804
- ro, kf, pm, rm, psm, rnm, rmls, ps: 3.41562085034638
- ro, kf, pm, rm, psm, rmls, rnm, ps: 3.409697544696715
- ro, rnm, kf, pm, rm, psm, rmls, ps: 3.426740547622089
- ro, kf, pm, rm, psm, rmls, ps, rnm: 3.132047932482537
- testには適用できなかった
- ラストサブはアンサンブルする
  - nb065
    -  remove low speedとposition shiftのパラメータ変えた
  - これとnb056(snap to girdあり)を0.05:0.09で混ぜる
    - cv: 3.137867005883899
    - lb: 5.914