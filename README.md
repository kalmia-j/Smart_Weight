# Smart Weight
　このプログラムは私が2年生1学期に「IoTとロボティックス」という授業でチームを組み、作成した”ウエイトトレーニング補助プログラム”です。補助と言っても物理的な補助ではなく、ウエイトトレーニングで重要な姿勢の矯正プログラムになります。python言語を利用し、”Pymodi”というモジュールを活用して実際のウエイトトレーニングの際に使えるような作品です。
## プロジェクトの背景
　核心アイデアは「安全で効率的な運動を提供する」です。近年コロナの影響や運動系ユーチューバーの増加に伴い韓国ではウエイトトレーニングが流行っており、男女関係なく体の管理に力を入れています。instagramではボディープロフィールというタグでの投稿が200万を超しており、鶏胸肉市場も年々大きくなっており、この先も成長していくという研究結果も出ているほどです。そういう時代の中で、重量のある器具を使ったウエイトトレーニングの際に怪我を負う方も少なくないですし、PTと呼ばれるトレーナーの補助を受ける際にかかる費用も大きいです。そこで考えついたのが、姿勢の矯正をアシストしながら、危ない状況には周りの人、もしくは緊急連絡先として登録した方に知らせることができる”Smart Weight”でした。時間もあまりなかったため、BIG3と言われるベンチプレス、スクワッド、デッドリフトに焦点を合わせ、作成しました。

## プロジェクトの進行方法
 　私の長所は”効率的な進行”と、軍隊の経験から培った”リーダーシップ”、そして”協調性”です。でかい内容を講義を聞いている生徒たちに発表し、このプロジェクトに興味がある生徒3人（開発1、デザイン2人）を選択、一緒に進行しました。開発は私と2人で担当し、発表資料、報告書等をデザインの2人に担当してもらいました。プロジェクトにおいて重要だと言われている役割分担では、私が土台となる姿勢矯正システムの関数を担当し、もう一人にはemergency状況を判別する関数をおおまかに作成してもらいました。その後にミーティングを数回行い、相互の関数における問題点やさらなるアイデア、発表資料に盛り込む内容、デザインなどについて話し合いました。ここでグループワークでの大きな問題である仕事の割り振りに対する個人の技術力、意欲の違いという壁に衝突しました。これの解決案として私は全体で集まるのではなく、1体1のミーティングを行うことにしました。一緒に開発を担当している方とはコミュニケーションの量を増やし、できそうな内容を把握、割り振っていきました。デザインの方に関しては発表材料の仮案を共有ファイルで作成し、フィードバックをしあいました。また、1対1のミーティングの際に各個人のスケジュールを把握することを大事にしていました。もちろんリーダーとしてチームメンバーを無理やり引っ張っていくことも重要ですが、自分の意見を押し付けるのではなく、相手の意見をしっかり聞きながらチームとしての活動、割り振る仕事を決めていくことによって効率的かつ円満なコミュニティーを維持できたと思っています。

## 使用したPymodiモジュール
  ### 使用言語
  - python言語
  ### ボタン
  - 押すことによってON, OFFを切り替えるモジュール 
  ### ダイアル
  - ダイアルで運動の種類を選択可能
  ### ジャイロスコープ
  - 器具の速度、加速度、振動数、器具の過度な傾きを測定
  - 設定した閾値を超す値が測定された場合緊急状態と判断
  ### 赤外線センサー
  - 近接する距離の感知
  - ベンチプレスなど体と器具の距離を判断する時に採用
  ### 超音波センサー
  - 遠距離の感知
  - スクワッド、デッドリフトで器具が床から遠くなる際に使用
  ### ディスプレイ
  - 姿勢の良し悪しをディスプレイに表示（o,x）
  ### マイク
  - 大きな音（悲鳴や器具が起こす音）を感知し、危険状況か判断
  ### スピーカー
  - 危険と判断された場合、音を出力

## 関数の説明
  ### 姿勢関数 : 運動回数の設定
  　格関数（Bench_Press, Squat, Deadlift）の始まりはほぼ同一であり、ベンチプレスは12回1セット、スクワッドは8回1セット、デッドリフトは10回1セットで設定し、3セット終わったらプログラム終了になります。
  ### 姿勢関数 : 測定点
  - 3運動とも角速度、傾き、振動数を測定
  - ベンチプレスは赤外線センサーを利用した距離測定関数distance()を追加、最初の測定点にのみ使用
  - デッドリフトは超音波センサーを利用した遠距離測定関数ultrasonic_distance()を追加、最初の測定点にのみ使用
  - 速度、傾き、振動、距離(ベンチプレスとデッドリフトモードのみ)がすべて正常範囲内に入った場合、測定点をTrueで通過
  - 上と下にある両測定点ともにTrueで通過時に1回をカウント
  ### 姿勢関数 : セット
  - 両測定点ともにTrue判定の場合、1回としてカウント
  - チャンスを与える回数を目標回数より多く与え、目標回数達成時にセット通過
  - TrueかFalseかに関係なく、回数を満たせば次のセットに移行
   ### 姿勢関数 : 最終結果出力
   - 3セット全て完了した後、3セット全てTrueでだった場合、ディスプレイには成功を出力
   - 3セットのうち1つでもFalseだった場合、失敗を出力
   ### 姿勢関数 : emergency状況
   - 過度な加速度、バーベルが過度な傾き、過度な震え、大きな音を検知した場合にemergencyと判断
   - マイクから「ピー」という音声出力
   - 危険状況を認識した場合、直ちに緊急電話連絡先に電話が来る機能を実現

