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


- [train/test]/[drive_id]/[phone_name]/[phone_name]_derived.csv - 生のGNSS測定値から派生したGNSS中間値で、便宜的に提供されています。

これらの派生値を使用して、補正された疑似距離（電話機から衛星までの幾何学的な距離の近似値）を次のように計算できます： correctedPrM = rawPrM + satClkBiasM - isrbM - ionoDelayM - tropoDelayM。ベースラインの位置は、補正されたPrMと衛星の位置を用いて、標準的な重み付き最小二乗（WLS）ソルバーを使用して計算されます。このソルバーには、各エポックの状態として、携帯電話の位置（x、y、z）、クロックバイアス（t）、および各固有信号タイプのisrbMが設定されています。

- collectionName - "grand "の親フォルダの名前です。

- phoneName - 親フォルダの名前。

- millisSinceGpsEpoch - GPSエポック(1980/1/6 midnight UTC)からのミリ秒単位の整数。

- constellationType。GNSSコンステレーションタイプ。constellation_type_mapping.csvで提供されるマッピング文字列値を持つ整数値。

- svid - 衛星ID。

- signalType - GNSS信号タイプは、コンステレーション名と周波数帯の組み合わせです。スマートフォンで測定される一般的な信号タイプは以下の通りです。GPS_L1、GPS_L5、GAL_E1、GAL_E5A、GLO_G1、BDS_B1I、BDS_B1C、BDS_B2A、QZS_J1、QZS_J5。

- receivedSvTimeInGpsNanos - チップセットが受信した信号の送信時間で、GPSエポック以降のナノ秒数で表します。ReceivedSvTimeNanosから変換すると、この派生値はすべての星座で統一された時間スケールになりますが、ReceivedSvTimeNanosはGLONASSでは一日の時間、GLONASS以外の星座では週の時間を参照します。

- [x/y/z]SatPosM - ttx = receivedSvTimeInGpsNanos - satClkBiasNanos (以下に定義)で定義される "真の信号送信時間 "の最良の推定値におけるECEF座標フレーム内の衛星位置(メートル)。これらは、衛星放送のエフェメリスを用いて計算され、真の衛星位置に対して約1mの誤差があります。

- [x/y/z]SatVelMps - 信号送信時刻(receivedSvTimeInGpsNanos)におけるECEF座標フレーム内の衛星速度(meters per second)。これらは、衛星放送のエフェメリスを用いて、このアルゴリズムで計算されます。

- satClkBiasM - 信号送信時刻(receivedSvTimeInGpsNanos)におけるハードウェア遅延を組み合わせた衛星時刻補正(メートル単位)。その時間に相当するものがsatClkBiasNanosと呼ばれています。

- satClkBiasNanosは、satelliteTimeCorrectionからsatelliteHardwareを差し引いた値に相当します。

- constellationType。GNSSコンステレーションタイプ。constellation_type_mapping.csvで提供されるマッピング文字列値を持つ整数値。

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
  
