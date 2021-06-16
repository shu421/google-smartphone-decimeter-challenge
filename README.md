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
- UncalAccel - Android APIのSensor#TYPE_ACCELEROMETER_UNCALIBRATEDから収集された、校正されていない加速度計からの読み取り値。
- utcTimeMillis - 以下のelapsedRealtimeNanosと、最近のNTP (Network Time Protocol)同期後のUTCでの推定デバイス起動時間の合計です。
- elapsedRealtimeNanos。イベントが発生した時間をナノ秒単位で表したもの。
- UncalAccel[X/Y/Z]Mps2 - [x/y/z]_uncalib（バイアス補正なし）です。
- Bias[X/Y/Z]Mps2 - 推定された[x/y/z]_biasです。以前の日付で収集されたデータセットではNullとなります。
- UncalGyro - Android APIのSensor#TYPE_GYROSCOPE_UNCALIBRATEDから収集された、キャリブレーションされていないジャイロスコープからの読み取り値
- utcTimeMillis - 以下のelapsedRealtimeNanosと、最近のNTP（Network Time Protocol）同期後のUTCでの推定デバイス起動時間の合計です。
- elapsedRealtimeNanos。イベントが発生した時間をナノ秒単位で表したもの。
- UncalGyro[X/Y/Z]RadPerSec - [X/Y/Z]軸周りの角速度（ドリフト補正なし）をrad/sで表したものです。
- Drift[X/Y/Z]RadPerSec - [X/Y/Z]軸周りの推定ドリフト量をrad/sで表したものです。以前の日付で収集されたデータセットでは無効です。
- UncalMag - Android APIのSensor#STRING_TYPE_MAGNETIC_FIELD_UNCALIBRATEDから収集された、校正されていない磁力計からの読み取り値です。
- utcTimeMillis - 以下のelapsedRealtimeNanosと、最近のNTP（Network Time Protocol）同期後のUTCでの推定デバイス起動時間の合計。
- elapsedRealtimeNanos。イベントが発生した時間をナノ秒単位で表したもの。
- UncalMag[X/Y/Z]MicroT - [x/y/z]_uncalib（バイアス補正なし）。
- Bias[X/Y/Z]MicroT - 推定された[x/y/z]_biasです。以前の日付で収集されたデータセットではNullです。
- OrientationDeg - 各行は、Android APIのSensorManager#getOrientationから収集された、デバイスの推定方位を表します。このメッセージは、2021年3月以降に収集されたログでのみ利用可能です。
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
# 20210617
- 橋の下だと精度悪くなる？
<img width="902" alt="スクリーンショット 2021-06-17 8 54 23" src="https://user-images.githubusercontent.com/71954051/122309689-b15a9d00-cf49-11eb-986f-96dc93bd3610.png">
- nb019とnb020もっと見る