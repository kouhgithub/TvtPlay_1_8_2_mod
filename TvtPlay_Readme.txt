﻿TVTest TvtPlay Plugin ver.2.8 + BonDriver_Pipe.dll + TvtAudioStretchFilter.ax

■概要
TVTest付属のBonDriver_UDPまたは添付のBonDriver_Pipeを使ってローカルTSファイルを
再生するプラグインです。

■動作環境
・Vista以降
・TVTest/TVH264 ver.0.7.16 以降。ver.0.8.1 以降を推奨
・必要ランタイム: 2015、2017、および 2019 用 Visual C++ 再頒布可能パッケージ
  ・ビルド環境: Visual Studio Express 2017 for Windows Desktop

■初期導入FAQ
○このReadme長いんだけど？
  とりあえず「使い方」の項目まで読むと良いとおもいます。あと、サクラエディタの
  F11キー等でいい感じにアウトラインが出るようです。
○再生26時間に1回ぐらい映像が乱れるのがイヤ
  「設定ファイルについて」→設定キーTsAvoidWraparound を参照
○BonDriver_UDP(Pipe)切り替え時に自動でプラグインを有効にしてほしい
  「TVTest起動オプションについて」を参照
○TVTest起動時にたまにフリーズする
  TVTest0.8.1以降を使ってください。使用できない場合は「設定ファイルについて」→
  設定キーRaiseMainThreadPriority を参照
○倍速再生時にしばらくフリーズすることがある
  TVTest0.8.1以降を使ってください。これ以前のTVTestではドロップしたTSファイルの
  倍速再生でデッドロックします。加えて倍速再生はなるべくBonDriver_Pipeを使ってく
  ださい。「倍速再生について」を参照
○しばらく再生していると映像が途切れとぎれになったりする
  設定キーTsUsePerfCounter/TsEnableUnderrunCtrl を参照
○シークがおそい
  TVTest設定の「バッファリングを行う」を使っている場合、かわりに設定キー
  TsReadBufferSizeKB を使用することをお勧めします
○レジュームは？
  設定キーFileInfoMax を参照
○チャプターは？
  「チャプター機能について」を参照

■以前のバージョンからの移行
TvtPlay.tvtpを以前のものと置きかえてください。必要に応じて(更新があれば)
BonDriver_Pipe.dllとTvtAudioStretchFilter.axも置きかえてください。設定ファイル
TvtPlay.iniは基本的にそのまま引き継げます。

■使い方
TVTestのPluginsフォルダにTvtPlay.tvtpを入れてください。BonDriver_Pipe.dllは
TVTest.exeのあるフォルダに入れてください。"x64"フォルダにあるものは64bit版の
TVTest向けです。
"plus"フォルダにあるTvtPlay.tvtpはTVTest0.9.0以降専用です。

TVTest起動オプションの最後に拡張子.ts .m2t .m2ts .mp4 .m3u .tslist いずれかのフ
ァイルパスを付加するとプラグインは有効になり、起動時にそのファイルを開きます。起
動オプションについては後述の「TVTest起動オプションについて」も参照してください。
例:
  TVTest.exe /d BonDriver_UDP.dll foo.ts
  # TVTestのデフォルトのBonDriverに"BonDriver_UDP.dll"を指定している場合、
  # /d BonDriver_UDP.dllは不要

起動するとTVTestウインドウ下部にコントロールが表示されます。デザイン的にTVTestの
ウインドウ枠を細くすることをお勧めします。コントロール左側の12個のボタンは左から
、ファイルを開く、リピート設定、シーク-60秒/-15秒/-5秒、一時停止、シーク5秒/15秒
/60秒、次のファイル・末尾シーク、倍速再生200%/50%、です。なお、「ファイルを開く
」ボタンを右クリックすると、TVTestの録画フォルダにある.tsファイルを簡易表示しま
す。また、「次のファイル・末尾シーク」を右クリックすると再生リストを表示します。
コントロール中央はシークバーになっていて、左クリックで任意の位置にシーク、右クリ
ックで再生を一時停止/再開します。コントロール右側には再生位置および総再生時間が
表示されます。

倍速再生を利用する方は音声フィルタの設定が必要です。倍速再生はBonDriver_Pipe.dll
の使用をお勧めします。同梱のDirectShowフィルタTvtAudioStretchFilter.axを適当なフ
ォルダに入れ、コマンドプロンプトから管理者権限でregsvr32 {フィルタのパス} と入力
してください(この辺は「DirectShowフィルタ 登録」あたりでググってください)。無事
登録されていれば、TVTestの設定->再生->音声フィルタにこのフィルタが現れるので、こ
れを選択してください。

*** 以下、必要に応じてお読みください ***

■BonDriver_Pipe.dllについて
BonDriver_UDPのプロセス間通信方式をパイプに改変したものです。転送中にドロップし
ないのと、UDP/IPのスタックを介さないので軽い(はず…)のが利点です。興味のある方は
使ってみてください。TVTestに対してはBonDriver_UDPのフリをするので、BonDriver_UDP
と同じ要領で使ってください。BonDriver_UDPと同時使用もできます。

■TVTest起動オプションについて
TVTest起動時につぎのようなオプションを追加することで、プラグインの有効・無効の自
動化などが可能です。設定キーTvtpCmdOptionでも指定できます【ver.1.5r3～】。
/tvtpudp          BonDriver_UDP.dllの使用の有無でプラグインを有効・無効にする
/tvtpudp /tvtplay 上述の動作＋ファイルパスの拡張子を問わない
/tvtpipe          BonDriver_Pipe.dllについて/tvtpudpと同様(同時指定もできる)
                  BonDriver_Pipe0.dll～BonDriver_Pipe9.dllにリネームされたものも
                  同様に扱います【ver.2.5～】。使用中のBonDriverによって挙動を変
                  えるプラグインをカスタマイズするときに便利です
/tvtplay          起動時にプラグインを有効化＋ファイルパスの拡張子を問わない
# 以上のオプションは複数起動禁止のTVTest起動中に追加できます(削除はできません)
/tvtpofs <ms>[+-]<ofs>
                  初期再生位置をミリ秒で指定する
                  例: /tvtpofs 30000      # 30秒の位置から再生
                      /tvtpofs 30000-3000 # 27秒の位置から再生
/tvtpstr [A-Z]    初期再生速度を指定する
                  例: /tvtpstr B          # 設定キーStretchBで倍速再生

■読み込み可能なプレイリストファイル
プレイリストファイル仕様はおおむねBonDriver_File+TVTestPluginに準拠します。拡張
子.m3uまたは.tslistをもつテキストファイルにファイルパスを絶対パスまたは相対パス
で列挙してください。'#'で始まる行はコメントとして無視します。また'"'の対応があれ
ば取り除くので、Vista以降の「パスとしてコピー」のコピペでもプレイリストになりま
す。読み込み可能な文字コードはUTF-16LE(Unicode)、UTF-16BE、UTF-8です。BOM(Byte
Order Mark)の無いものはUTF-8と判断します。

■倍速再生について
倍速再生は音声を伸び縮みさせることで実現しています。動画をスキップさせているわけ
ではないので、速度に応じてシステムの負荷が高まります。また、BonDriver_UDP.dllを
使っている場合や、動画にドロップやエラーのある箇所では、あまり高速に再生すると数
秒～数十秒間TVTestが固まることがあるかもしれません。しばらく放っておけば戻ります
が、あまり再生速度を上げすぎないように注意してください。
また、倍速再生に使用するTvtAudioStretchFilterですが、倍速再生時以外は何も処理を
しない(Waveデータを通過させるだけ)ので、このフィルタを設定するだけで負荷が上がっ
たりすることはおそらく無いです。

■TvtAudioStretchFilterの追加機能
ver.1.3以降に添付のフィルタは5.1ch音声に対応しています。S/PDIFパススルーには未対
応なので注意してください。
TVTestの音声フィルタを一つしか指定できないことへの対策として、ver.0.9r3以降に添
付のTvtAudioStretchFilterでは、接続するDirectShowフィルタを1つだけ指定できるよう
になりました。たとえば、フィルタを置いたフォルダに"TvtAudioStretchFilter.ini"を
次のような内容で作成すると、ffdshowがインストールされていて、"Uncompressed"形式
が有効に設定されていれば、フィルタの後続にffdshow Audio Decoderが接続されます。

[TvtAudioStretchFilter]
AddFilter={0F40E1E5-4F79-4988-B1A9-CC98794E6B55}

(エラーメッセージ)
  「追加のフィルタを生成できません。」
  AddFilterに指定したDirectShowフィルタが見つからなかったときに表示されます。

(上級者向け)
  一般に1入力1出力のPCM音声フィルタであれば(おそらく)何でも追加できます。
  AddFilterにはDirectShowフィルタのクラス識別子(CLSID)を指定してください。CLSID
  はGraphEditを使うか、レジストリキーHKEY_CLASSES_ROOT\CLSID内部をフィルタ名(
  FriendlyName)で検索すれば取得できます。
  フィルタをどのように追加するかは、GraphEditで観察するとよく分かると思います。

■容量確保録画中の追っかけ再生について
ファイル末尾(2秒ぐらい)に有効なTSデータがないとき、そのファイルはEpgDataCap_Bon
やRecTaskのようなファイル容量確保型の録画中であると判断します。このようなファイ
ルの追っかけ再生時にはコントロールの総再生時間の右隣に'*'が表示されます(通常の追
っかけ再生は'+')。また、総再生時間はファイルサイズが初めて減少するタイミングまで
増加し続けます。さらに、「末尾シーク」ボタンや再生時間ぎりぎりの位置へのシークは
機能しない制約があります。

■設定ファイルについて
設定ファイル"TvtPlay.ini"は初回使用時プラグインフォルダに自動作成されます。必要
な場合はこれを直接編集してカスタマイズしてください。以下は設定キーの説明です。

Version【ver.1.0～】
    設定ファイルのバージョン
    # デフォルト値を出力するために使います。特にユーザがいじる必要はありません。
    # あるキーを削除する(=デフォルト値にもどす)ときは、同時にこのキーも削除して
    # おくと、デフォルト値を再出力できます。
TvtpCmdOption【ver.1.5r3～】
    追加の起動オプション
TsRepeatAll / TsRepeatSingle【ver.1.0～】
    再生リスト全体/1ファイルをリピート再生する[=1]かどうか
TsRepeatChapter【ver.1.4～】
    チャプターリピートする[=1]かどうか
    # 現在再生中の開始チャプターと終了チャプター(名前が"i"または"o"で始まるもの)
    # の間をリピート再生します。
TsSkipXChapter【ver.1.4～】
    チャプタースキップする[=1]かどうか
    # スキップチャプター(名前が"ix"または"ox"で始まるもの)の間をスキップします。
TsReadBufferSizeKB【ver.1.6～】
    TSファイルの読み込みバッファをキロバイト単位で指定
    # [=0]とするとバッファリングしません(従来動作です)。
    # 最大で36MB[=36864]程度まで指定できます(それより大きくしてもこのサイズ以上
    # は確保しません)。
TsSupposedDispDelay【ver.1.5～】
    TSファイルの読み込みから映像表示までの遅延時間(ミリ秒)(最大値は[=5000])
    # 以下の挙動に影響します:
    # ・再生終了後、次のファイルの再生開始まで{設定値+300}ミリ秒だけ待つ
    # ・チャプターを追加するときに設定値だけ追加時刻を巻きもどす
    # ・チャプタースキップやチャプターリピートの開始・終了を設定値だけ遅らせる
    # デコーダに依存しますが、おおまかにTVH264なら大きめ(2秒ぐらい?)、TVTestの「
    # バッファリングを行う」を指定している場合は適宜設定すれば、端切れせずに再生
    # できます。
TsResetAllOnSeek
    シーク時にビューアリセットではなく全体リセットをかける[=1]かどうか
TsResetDropInterval【ver.0.5～】
    シーク後にドロップカウントをリセットするために監視する間隔(ミリ秒)、またはリ
    セットしない[=0]
    # シーク後、ドロップカウントの増加が止まれば値をリセットします。
    # 後述の「ドロップカウントについて」も参照
TsThreadPriority【ver.0.7～】
    TSデータの転送スレッドの優先度
    # 通常は変更する必要はありません。
    # システム高負荷時に音声がとぎれたりする場合には、"TVTest.ini"設定ファイルの
    # StreamThreadPriority(「ストリームスレッドの優先度」の設定値)と同じ値にする
    # と改善するかもしれません。
TsStretchMode【ver.0.8～】
    倍速再生モードを指定
    # [=1]再生速度だけ調整します(速度に応じて音が高くなったりします)
    # [=2]ピッチだけ上げ下げします(用途不明)
    # [=3]ピッチと再生速度の両方を調整します
TsStretchNoMuteMax / TsStretchNoMuteMin【ver.0.8～】
    倍速再生の速度が設定値以上/以下なら無音にする
TsConvTo188【ver.0.9～】
    192ByteTS(Timestamped TS)の場合、188ByteTSに変換して転送する[=1]かどうか
TsEnableUnderrunCtrl【ver.1.8～】
    BonDriverのバッファアンダーランを防ぐ[=1]かどうか
    # 映像が途切れとぎれになる場合に解決策になるかもしれません。ただし、TVTestの
    #「バッファリングを行う」は使わないでください。
    # BonDriver_Pipe使用時のみ効果があります。
TsUsePerfCounter【ver.1.3r2～】
    パフォーマンスカウンタをTSデータ送出タイミングの基準とする[=1]かどうか
    # 基本的に[=1]で良いと思います。
    # 詳細は後述の「設定キーTsUsePerfCounterについて(上級者向け)」を参照。
TsAvoidWraparound【ver.1.1～】
    ラップアラウンドを避けるようPCR/PTS/DTSタイムスタンプを変更するかどうか
    # DirectShowのバグによってタイムスタンプが巡回する瞬間に映像が乱れるのを防ぎ
    # ます。Microsoftがこの問題を修正する(かわかりませんが…)までは利用をお勧め
    # します。
    # [=1] ファイル読み込み部分で修正します(従来動作です)。ファイルがスクランブ
    #      ルされているときは正しく再生できません。
    # [=2] ストリームコールバックを利用して修正します【ver.2.0～】。ファイルがス
    #      クランブルされていても再生できます。TVTest0.8.0以降で利用可能です。
    # この設定は再生位置の表示部分を右クリックで変更できます。
TsPcrDiscontinuityThreshold【ver.1.5r2～】
    カット編集部分の検出しきい値(ミリ秒)
    # PCR(Program Clock Reference)値がこのしきい値以上飛んでいる部分をカット編集
    # (またはドロップ)部分とみなしてビューアリセットをかけます。
    # 最小値は[=200]、または[=0]とすると検出しません。
    # 正常なTSファイルであれば、PCRは規定で100ミリ秒以下の間隔で埋め込まれます。
TsTryGaplessPause【ver.2.2～】
    ギャップなしで一時停止/再生する[=1]かどうか
    # フレームが飛ぶことなく再生再開できます。デフォルト[=0]ですがオススメです。
    # 音声フィルタにTvtAudioStretchFilterを指定している場合だけ効果があります。
    # バッファ設定(TsEnableUnderrunCtrl)等によっては機能しない場合があります。
ShowOpenDialog【ver.1.2～】
    プラグイン有効時に「ファイルを開く」ダイアログを表示する[=1]かどうか
    # /tvtpudp /tvtpipeと組みあわせて利用するとTVTestのチャンネル選択画面などで
    # 便利かもしれません。
RaiseMainThreadPriority【ver.1.5r3～ TVTest0.8.1以降は廃止】
    プラグイン有効かつ起動時にTVTestの主スレッド優先度を少し上げる[=1]かどうか
    # TVTest起動時に数秒～数十秒間フリーズする現象がみられる場合は試してみてくだ
    # さい。
AutoHide【ver.1.1～】
    通常表示時にコントロールを隠す[=1]かどうか
    # この設定は再生位置の表示部分を右クリックで変更できます。
    # RowPos[=0]のときは最大化表示のときだけ隠します【ver.1.5～】。
RowPos / RowPosFull【ver.1.1～】
    コントロールを表示する高さを指定
    # [=0]でTVTestウインドウの下、[=1]でステータスバーの位置、[=-1]でタイトルバ
    # ーの位置になります。実用的なのは[=-2]～[=2]の範囲だと思います。
    # RowPosFullの[=0]は[=1]と同じです。
SeekMode【ver.1.9～】
    シークバーのシーク方法を指定
    # [=0]: 左クリック押下時にシーク(従来)
    # [=1]: ドラッグ後にシーク
    # [=2]: ドラッグ中にシーク(シーク中のディスクアクセスが増えるので注意)
DispOffset【ver.1.2～】
    シークバーに再生位置からの相対位置を表示する[=1]かどうか
DispTot【ver.0.5～】
    シークバーに放送時刻を表示する[=1]かどうか
DispTotOnStatus【ver.0.5～】
    再生位置の表示の隣に放送時刻を表示する[=1]かどうか
    # この設定は再生位置の表示部分を右クリックで変更できます【ver.1.2～】。
SeekItemMinWidth【ver.1.6～】
    シークバーの最小ピクセル幅
StatusItemWidth【ver.0.5～】
    再生位置の表示部分のピクセル幅、または自動調整する[=-1]
TimeoutOnCommand / TimeoutOnMouseMove
    キーコマンド/マウス操作をおこなった後にコントロールを表示する時間をミリ秒で
    指定、または表示しない[=0]
Salt【ver.0.7～】
    0～65535のランダムな値
    # 特にユーザがいじる必要はありません
FileInfoMax【ver.0.7～】
    レジューム情報を記録する最大数、または記録しない[=0]
    # 扱うTSファイルの数より大きくしておけば忘れられることはないでしょう
FileInfoAutoUpdate【ver.1.7～】
    再生中、定期的(5秒間隔)にレジューム情報を書き出す[=1]かどうか
PopupMax【ver.0.8～】
    フォルダの簡易表示機能でファイルを一覧表示する最大数、またはこの機能を使わな
    い[=0]
PopupDesc【ver.0.9～】
    簡易表示機能でファイルを降順表示する[=1]かどうか
PopupPattern【ver.0.8～】
    簡易表示機能で表示するフォルダを指定
    # たとえば[=D:\media\*.ts]とすれば"D:\media"フォルダにある.tsファイルが名前
    # 順にポップアップ表示されます。
    # %RecordFolder% はTVTestの録画フォルダに置き換わります。
ChaptersFolderName【ver.1.7～】
    TSファイルのある場所にこの名前のフォルダがあれば、チャプターファイルをここか
    ら読み書きする
    # 指定しなければこの機能を無効にします。
    # 任意の名前にできますが、[=chapters]以外は非推奨です。
SeekItemOrder / StatusItemOrder【ver.1.6～】
    シークバー / 再生位置の表示部分 のButton[00-17]に対する順序
    # たとえば[=3]とすればButton03の左隣、[=0]とすれば左端に配置されます。
IconImage
    アイコン画像を絶対パスかTVTestフォルダからの相対パスで指定
    # プラグインに内蔵されている"src/Buttons.bmp"の画像を置き換えます。モノクロ
    # BMPファイルを指定してください。
Seek[A-Z]
    シーク時間をミリ秒で指定する
    # ボタンコマンドやキー割り当ての"シーク:A"～に対応します。
    # デフォルトはSeek[A-H]ですが、Zまで指定できます【ver.1.4～】。
Stretch[A-Z]【ver.0.8～】
    倍速再生の速度を、等速にたいする比率(パーセント)で指定する
    # [=25]～[=800]まで設定可能です。
    # デフォルトはStretch[A-D]ですが、Zまで指定できます【ver.1.4～】。
Button[00-17]
    ボタンのアイコン番号とコマンドを指定する
    # フォーマット(全体で191文字まで。[]は省略可):
    #   {アイコン番号}[,Width=(ボタン幅)][,{右クリックコマンド}],{コマンド}
    # コマンドはキー割り当て順に Open,OpenPopup,ListPopup,Close,Prev,SeekToBgn,
    # SeekToEnd,SeekToPrev,SeekToNext,AddChapter,RepeatChapter,SkipXChapter,
    # ChapterPopup,Loop,Pause,Stretch,StretchRe,StretchPopup,Seek[A-Z],
    # Stretch[A-Z]
    # (ボタン幅)のデフォルトはアイコン画像の高さ(前述IconImageキー参照、内蔵のも
    # のは16)です。';'でコメントアウトするか何も指定しなければそのボタンは消えま
    # す。並べ替えもできます。Stretch/StretchReコマンドは設定キーStretchAから
    # [=100]となる設定キーまでを順/逆順に倍速切り替えします。
    # アイコン画像の X座標=(アイコン番号)×(画像の高さ) の位置をアイコンとして使
    # 用します。ただし、左上1pxが黒色ならば(画像の高さ)pxを最大とする可変ピッチ
    # です【ver.0.9～】。
    # RepeatChapter,SkipXChapter,Loop,Pause,Stretch*は切り替えのため複数のアイコ
    # ンを使います。
    # "{アイコン番号}"のフォーマットについて【ver.0.9～】
    # ・アイコン番号を複数指定(-でつなげる)して、アイコンを合成できます。
    # ・'があれば、続く1文字のASCIIコードをアイコン番号に置き換えます。
    # ・~があれば、以降を画素反転して合成します。
    # ・:があれば、以降は切り替え時のアイコンを合成します。
    # 例:
    #   [ +5m] '+'5-59
    #   [-300] '-23-20-20
    #   [ 35%] '3'5'%:30~'3'5'%
    #   [x1.0:x4.0:x2.0:x0.5:x0.2]
    #          '*'1' '.'0:30~'*'4'.'0:30~'*'2'.'0:30~'*'0'.'5:30~'*'0'.'2
    #   ※後述「ボタンカスタマイズ例」も参照。ほかに「TvtPlayBtn.bmp」(後述「ソ
    #     ースについて」参照)の説明がわかりやすいです。

[FileInfo]セクション【ver.0.7～】
    レジューム情報のリスト
    # TSファイルの先頭2キロバイトとSaltから得られたハッシュ値と、そのファイルの
    # レジューム位置(ミリ秒)の対応データが書き込まれます。
    # 対応データは使われなかった順に消えていきます。
    # 元のTSファイルがなければ、どのようなファイルを開いたかを特定されることはあ
    # りません。

[MP4]セクション【ver.2.4～】
Enabled
    MP4再生機能(後述)を有効にする[=1]かどうか
VttExtension【ver.2.7～】
    同時に読み込むWebVTTファイルの拡張子
    # デフォルトは[=.vtt]です。空文字のときは無効にします。
    # https://github.com/xtne6f/b24tovtt が出力したWebVTTを利用できます。MP4ファ
    # イルと同名でこの拡張子のファイルがあれば読み込みます。
    # https://github.com/xtne6f/TVCaptionMod2 ver.2.5以降など、UCS(UTF-8)形式字
    # 幕に対応したプラグインなどで字幕表示できます。
PsiDataExtension【ver.2.8～】
    同時に読み込むPSI/SIデータファイルの拡張子
    # デフォルトは[=.psc]です。空文字のときは無効にします。
    # MP4再生時に番組情報や放送時刻、データ放送といった情報を再現できます。
    # https://github.com/xtne6f/psisiarc が出力した書庫を利用できます。
    # BroadcastIDやTimeキーの指定は無視されます。
BroadcastID
    ファイルの放送IDを指定
    # NetworkID=0x0001,TransportStreamID=0x0002,ServiceID=0x0003としたいときは
    # [=0x000100020003]のように指定します。
Time
    ファイルの放送時刻を指定
    # [=2000-01-01T09:00:00]のように指定します。空文字のときはファイルの更新時刻
    # を放送時刻とします。
Meta
    メタ情報を指定する設定ファイルの名前
    # デフォルトは[=metadata.ini]です。空文字のときは常に[MP4]セクションの指定に
    # 従います。MP4ファイルのあるフォルダにこの設定ファイルがあれば、BroadcastID
    # とTimeの指定を上書きします。設定ファイルの内容は[*](全てのファイルに適用)
    # か[ファイル名]をセクションとして以下のように記述します:
    #   [*]
    #   BroadcastID=0x111122223333
    #   [foo.mp4]
    #   Time=2011-11-11T11:11:11
    #   [bar.mp4]
    #   BroadcastID=0x444455556666
    #   Time=2012-12-12T12:12:12
    #   ※foo.mp4はBroadcastID=0x111122223333,Time=2011-11-11T11:11:11
    #     bar.mp4はBroadcastID=0x444455556666,Time=2012-12-12T12:12:12になる

[ColorScheme]および[Style]セクション【ver.0.9～】
    セクションがあれば、デザインをプラグインで個別に設定する
    # TvtPlayは、起動時に"TVTest.ini"からこれらのセクションを読みこんでコントロ
    # ールのデザインを設定していますが、これを個別に設定できます。おもにTVTest本
    # 体の仕様変更への保険を目的としています。

[Status]セクション【ver.1.0～】
    セクションがあれば、フォントをプラグインで個別に設定する
    # 前述と同様に仕様変更への保険を目的としています。以下4つのキーを使います:
    # FontName / FontSize / FontWeight / FontItalic

■MP4再生機能について【ver.2.4～】
MP4ファイルをトランスポートストリームに変換してTVTestに送ることができます。H.264
に対応したTVTest(おそらく0.9.0以降に限定)はこれを再生できます。今のところ以下の
制限があります:
・形式は1映像(H.264)+1～2音声(AAC)のみ
・PAR情報は無視
ほか、特殊な(一般的でない)構造のMP4は再生できないかもしれません。また、キーフレ
ームの少ない映像はシーク後の乱れが長く続きます。

■ドロップカウントについて
シークを行うとTVTestステータスバーのドロップカウントは増加します。これはTSパケッ
トの巡回カウンタ(continuity counter)が不連続になるためで、正常な動作です。
また、この巡回カウンタは重畳するパケットのPIDごとにカウントアップしていくので、
送出頻度の低いパケット(EITやTOTなど)はドロップ判定が遅れます。シーク数秒～十数秒
後にドロップカウントがいくらか増えることがあるのは、おそらくこれが原因です。この
挙動が気持ちわるいひとはTsResetAllOnSeekを1にするかTsResetDropIntervalを大きめの
値(2000～4000程度)にしてみてください。

■設定キーTsUsePerfCounterについて(上級者向け)
一部の環境で GetTickCount と QueryPerformanceCounter の2つのAPIの計時結果(瞬間的
なものではなく数分～数時間単位でのもの)にずれが生じるようです(ウチの環境がそうで
したorz)。TvtPlayはTSデータをPCR(Program Clock Reference)の示すクロックに同期さ
せてTVTestに送るので、計時結果のずれが大きくなると音声が途切れとぎれになったりす
る可能性があります。
当該設定キーの設定値を変更すると、TvtPlayが基準にするAPIを、[=0]なら従来通りの
GetTickCount、[=1]ならQueryPerformanceCounterにします。どちらでも負荷は基本的に
同じです。DirectshowのレンダラはQueryPerformanceCounterを基準としているはずなの
で、ほとんどの環境で[=1]が適切だと思います。

■チャプター機能について
[ver.1.6からの変更点]
・chaptersフォルダがあれば利用するようになりました:
  <チャプターを読みこむ優先順位>
    1. chaptersフォルダの.chapter
    2. TSファイルのあるフォルダの.chapter
    3. chaptersフォルダの.chapter[s].txt
    4. TSファイルのあるフォルダの.chapter[s].txt
    5. TSファイル名に含まれるチャプター
  <.chapterファイルを書きこむ優先順位>
    1. 読みこんだ.chapter
    2. chaptersフォルダの.chapter
    3. TSファイルのあるフォルダの.chapter
  ※必須ではないですが、外部プログラム開発者は、もし対象のTSファイルのある場所に
    "chapters"という名前のフォルダがあれば、そこでチャプターファイルを読み書きす
    るようにすると便利だと思います

[ver.1.5からの変更点]
・チャプターのルールが少し変わりました:
  <旧>名前が"ix"と"ox"のときだけスキップチャプターになる
  <新>名前が"ix"と"ox"で始まるときスキップチャプターになる
  <旧>"i"と"o"で始まる同名のチャプター(例:"iTest"-"oTest")のみが、開始/終了チャ
      プターの対になる
  <新>"i"と"o"で始まれば開始/終了チャプターの対になる(例:"iTest1"-"oTest2")
  # おおざっぱにいうと、開始/終了チャプターの入れ子は使い道がなさそうなのでルー
  # ルからとりのぞいた感じです

[ver.1.4からの変更点]
・読み込む.chapterファイルの命名ルールが変わりました:
  <旧>foo.ts→foo.ts.chapter <新>foo.ts→foo.chapter
・OGMスタイルのチャプターファイル読み込み(書き込みはできない)に対応しました。前
  項の.chapterファイルがないとき、つぎの命名ルールのファイルを読み込みます:
  foo.ts→foo.chapter[s].txt
・右クリックでチャプター名を変更できるようになりました

[使い方]
まず、キー割り当ての「チャプターを追加」を適当なキーに割り当ててください。このキ
ーをつかって現在の再生位置にチャプターをつけることができます。チャプターをつける
と、開いているTSファイルの名前に.chapterを付加したファイル(中身はテキストファイ
ル)が作られ、チャプター情報が保存されます。また、シークバー上にはチャプターを表
す三角形が表示されます。この三角形をクリックするとチャプター位置にシークし、右ク
リックすると簡単なチャプターの編集ができます。「次のチャプター(SeekToNext)」や「
チャプターリピート/しない(RepeatChapter)」などをボタンに割り当てておくと便利かも
しれません。
ちなみに、.chapterファイルの中身の文字列をTSファイル名のどこかにコピペすると、
.chapterファイルなしでチャプター読み込みできます。

[上級者or開発者向け]
.chapterファイルを直接編集すれば、チャプターに名前をつけたり、動的なチャプターづ
けができます。チャプター機能の詳細についてはソースコード"src/_chwrite_test.cpp"
のコメントも参考にしてください(今後、仕様がかわるかもしれません)。

■仕様
・TVTest ver.0.7.20以前では、シーク後に音無しor映像無しになることがあります。当
  方ではシーク200回のうち7回発生しました。発生したときはビューアリセットしたりも
  う一度シークしたりしてください。
・総再生時間の表示は数秒以内の誤差がでる場合があります。また、総再生時間はファイ
  ル先頭および末尾の情報から算出した値なので、カット編集されている場合は不正確に
  なります
・26.5時間以上のTSファイルはおそらく正しく再生できません(Program Clock Reference
  が巡回する関係上)
・再生位置から一度に13.2時間以上離れた位置にシークすると、誤った位置に飛びます
・追っかけ再生に対応しています
・追っかけ再生中の総再生時間は推計です
・勢いでつくったので不具合もけっこうあるかも

■ソースについて
当プラグインの作成にあたり、EpgDataCap_BonのEpgTimerPlugInを参考にしました。また
、PCR/PTS/DTSタイムスタンプの変更コードの作成にあたり、TsTimeKeeper Ver 3.4.15.0
を参考にしました。
TSファイルの同期・解析のために、tsselect-0.1.8(
http://www.marumo.ne.jp/junk/tsselect-0.1.8.lzh)よりソースコードを改変利用してい
ます。
TvtAudioStretchFilterフィルタは、再生レート制御のために、SoundTouchライブラリver
.1.9.2(http://www.surina.net/soundtouch/)を利用し、TvtPlayスレ2>>774のSSE2最適化
パッチ(http://toro.2ch.net/test/read.cgi/avi/1348364114/774 )を少し補正したもの
を適用しています。自ビルドする場合は、TvtAudioStretchFilter.slnをビルドする前に
baseclasses/BaseClasses_VC14.sln と soundtouch/source/SoundTouch/SoundTouch.sln
をビルドしてください。
デフォルトのアイコン画像"Buttons.bmp"は、「HDUS関係ファイル置き場」(
http://2sen.dip.jp/dtv/)のup0598.zip「非公式 TvtPlayシークボタンカスタマイズ用ア
イコン 修正2」のデザインをもとに作成しています。61～65番のアイコンはup0635.zip
「TvtPlayBtn.bmp - 標準のボタン・文字を小さめに書き換えてシーク・倍速再生・チャ
プター関係のボタンを追加したアイコン画像 (txt加筆修正)」を使用しています。
チャプター選択ポップアップ機能はTvtPlayスレ>>115の人のパッチ(
http://toro.2ch.net/test/read.cgi/avi/1348364114/115 )を使用しています。
プレイリストのシャッフル機能はmike氏(https://github.com/mike2 )のコミットを使用
しています。
また、おもにTVTest ver.0.7.23からソースコードを流用しています。特に以下のファイ
ルはほぼ改変なしに流用しています:
  "Aero.cpp"
  "Aero.h"
  "BasicWindow.cpp"
  "BasicWindow.h"
  "ColorScheme.cpp"
  "ColorScheme.h"
  "DrawUtil.h"
  "DrawUtil.cpp"
  "Settings.cpp"(From: ver.0.7.21r2)
  "Settings.h"(From: ver.0.7.21r2)
  "StatusView.cpp"
  "StatusView.h"
  "Theme.cpp"
  "Theme.h"
  "WindowUtil.cpp"
  "WindowUtil.h"
ソースコメントに流用元を記述しています。流用部分については流用元の規約に注意し、
その他の部分は勝手に改変・利用してもらって構いません。

■更新履歴
ver.2.3 (2016-02-09)
・ソース管理をGitHubに移行した。以後の更新内容は省略
・https://github.com/xtne6f/TvtPlay
ver.2.2 (2014-02-15)
・SoundTouchライブラリの更新に追従
・TvtAudioStretchFilterについて、5.1chのダウンミックス再生がオフのときch変化で無
  音になることがあるのを修正
・ギャップなしで一時停止/再生できる機能を追加(設定キーTsTryGaplessPause)
・再生中ファイルのリネームや削除に対応
ver.2.1r2 (2013-08-21)
・ver.2.1で全画面表示でのDrag&Dropができなくなったのを修正
  ・スレ指摘感謝です。その昔ver.1.2でも同じことをやらかしてたりする
・ver.2.1でx86版のビルド環境にATL/MFCのセキュリティ更新が適用されていたのを修正
  ・ATL/MFCは不使用なのでKB971090、KB2538218のアンインストールで対応
  ・昨年11月ごろのビルド環境再インストールで紛れ込んだっぽい…
ver.2.1 (2013-08-11)
・ポップアップでチャプター選択してシークするコマンドChapterPopupがついた
・プレイリストのランダムシャッフル機能(その他の操作→シャッフル)がついた
・他プラグインに情報提供するウィンドウメッセージを追加
  ・同梱の"src/_getinfo_test.cpp"を参考のこと
・ファイル読み込みに線形アクセスのヒントフラグ(FILE_FLAG_SEQUENTIAL_SCAN)を付加
  ・とくにネットワークドライブからの読み込みが効率化する(はず)
・Drag&Drop処理が他プラグインと干渉しないように修正
・その他コードの整理
ver.2.0 (2012-09-26)
・TVTest0.8.0以降について、スクランブルされたTS再生でもラップアラウンド回避(設定
  キーTsAvoidWraparound)を利用できるようにした
・リサイズ時のちらつきを軽減
・スレッド作成についてバグ(になるかもしれない部分を)修正
・再生開始部分のコードを少しスマートにした
ver.1.9r3 (2012-07-12)
・<高速鑑賞機能>参照するPCRを間違えて字幕区間のタイミングがずれる可能性を排除
・<高速鑑賞機能>設定キーSlowerWithCaptionShowLate/ClearEarlyのデフォルトを変更
・計算量の多い部分の最適化+冗長コード整理
ver.1.9r2 (2012-06-19)
・StretchPopupを左クリックのボタンコマンドにしたときのアイコン画像処理を忘れてい
  たので修正
・↑以外の修正はないので、面倒なら1.9→1.9r2には置きかえなくてもいい
ver.1.9 (2012-06-18)
・設定キーSeekModeを追加し、デフォルトでドラッグシークにした
・起動時に倍速指定するオプション/tvtpstrを追加
・ポップアップで倍速指定するコマンドStretchPopupを追加
・つぎのファイルを開くときも倍速再生の状態をそのままにするようにした
ver.1.8r2 (2012-05-11)
・「ラップアラウンドを避ける」(設定キーTsAvoidWraparound)を使っている場合、特定
  パターンのパケットのPTS、DTSタイムスタンプを書き換えていなかったバグを修正
  ・特定位置で字幕プラグインの字幕が残ったままになったりしていたかもしれない
・高速鑑賞機能の差分ソースコードをプロジェクトにマージ(ただし機能オフでビルド)
ver.1.8 (2012-03-29)
・某機能実装にむけてBonDriver_Pipeの倍速再生への耐性を強化。関連する設定キー
  TsEnableUnderrunCtrlを追加
ver.1.7 (2012-03-17)
・簡易表示機能(設定キーPopupMax)について、100個をこえるファイルを扱うときの制限
  をとり除いた
・必要のないレジューム情報を記録しないようにした
・レジューム情報を定期的に書き出せるようにした(設定キーFileInfoAutoUpdate)
・"chapters"フォルダにチャプターファイルをまとめられるようにした
・再生中に参照するPCRが入れ替わる可能性を下げた
・up0635.zip「TvtPlayBtn.bmp」よりチャプターリピート/スキップアイコンを流用させ
  てもらった
ver.1.6 (2012-02-18)
・設定キーTsReadBufferSizeKBでファイル読み込みバッファ量を指定できるようにした
  ・思ったより効果的だったのでデフォルトで2048KBとした
・設定キーSeekItemMinWidthでシークバーの最小幅を指定できるようにした
・設定キーSeekItemOrder/StatusItemOrderでシークバーなどを移動できるようにした
・開始/終了チャプターの命名ルールを少し変更(既存のツールに影響はないはず)
・チャプターリピート/スキップの精度向上
ver.1.5r3 (2012-02-03)
・ver.1.5r2で、13.2時間以上のTSファイルの総再生時間がおかしくなる(はず)のを修正
・TVTest起動時まれに数秒～数十秒間フリーズする現象への対策として、設定キー
  RaiseMainThreadPriorityを追加
・ついでに設定キーTvtpCmdOptionを追加
ver.1.5r2 (2012-01-22)
・ver.1.5で、TsTimeKeeper等できれいにカット編集された部分まで検出してビューアリ
  セットしていたのを修正。関連する設定キーTsPcrDiscontinuityThresholdを追加
・ver.1.5で、設定値が引用符で囲われている場合に取り除いてなかったのを修正(つまり
  通常のiniファイルの仕様に戻した)
ver.1.5 (2012-01-20)
・人柱版表記をはずした(人柱さん達に感謝)。チャプター周辺は今後激しく変化するかも
・RowPos=0のとき、最大化表示のときにコントロールを隠せるようにした
・全画面表示からキー割り当てで最小化するとコントロールが表示されてしまうのを修正
・ドロップやカット編集への耐性強化
・設定キーTsWaitOnStopをTsSupposedDispDelayに変更(吸収)
・"Buttons.bmp"の旧[-60][+60]アイコンをつぶして3つ追加
・OGMスタイル(chapter.aufスタイル)のチャプターファイルの読み込みに対応
・ほか、チャプター機能の細かい機能追加(チャプターのini保存機能はとりあえず保留)
・ini読み書きを効率化
・Readmeのスペルミス修正(OpenPupup→OpenPopup)
ver.1.4 (2011-12-30)
・ボタンを18個にしてコマンド指定の自由度を上げた
・SeekとStretchを26個まで増やせるようにした
・逆順倍速切り替え(StretchRe)を追加した
・チャプター機能(インフラ？)を実装してみた
・TvtAudioStretchFilterのビルドオプションを改善してリビルド
  ・/OPT:NOWIN98でサイズを小さくしただけ、置きかえの必要なし
ver.1.3r3 (2011-12-15)
・ver.1.3以降、再生レート変化の激しい部分で再生が詰まる問題を修正
・ver.1.3以降、再生レートが極端に小さいデータ(ワンセグのみ等)のシークが失敗する
  可能性を軽減
ver.1.3r2 (2011-12-13)
・計時APIをQueryPerformanceCounterに変更し、設定キーTsUsePerfCounterを追加
ver.1.3 (2011-12-10)
・TvtAudioStretchFilterを5.1ch対応にした
・容量確保録画の追っかけ再生でのシーク失敗時の挙動を改善
・ファイルの読み出し単位を上げ、関連するコードを整理
・シーク動作のパフォーマンス改善
・メッセージポスト部分の微バグ修正(送り過ぎてた)
ver.1.2r2 (2011-11-24)
・BonDriver_Pipeの以下の不具合修正
  ・ドライバの初期化と再生開始のタイミングが重なった場合に、TVTestをフリーズさせ
    てしまうことがある
・全画面表示でドラッグ&ドロップできるように戻した
・TVTest待機状態(常駐モード)を考慮した
ver.1.2 (2011-11-22)
・「ファイルを開く」ダイアログを複数選択可能にしてプラグイン有効時に表示できるよ
  うにした
・再生位置の表示幅(設定キーStatusItemWidth)をデフォルトで自動調整するようにした
・シーク位置の描画方法をすこし変更した
・設定キーDispOffsetを追加した(個人的な需要)
ver.1.1r2 (2011-11-10)
・以下3点を修正した
  ・アダプテーションフィールド長の計算ミスにより、高頻度でPTS/DTSが書き換えられ
    ない(該当箇所の指摘ありがとうございます)
  ・複数起動禁止での起動時に1番目のコマンドオプションが無視される
  ・マルチディスプレイ環境で、プライマリ以外での全画面表示状態でプラグインを有効
    にすると、コントロールがプライマリに表示される(修正できたか不明)
ver.1.1 (2011-11-09)
・以下2点を修正した(バグ報告サンクスです)
  ・再生リストが1つだけのときに全体リピートが働かない
  ・「ファイルを開く」ダイアログ使用後にカレントディレクトリが変更されてしまう
・コントロールの位置をカスタマイズできるようにした
・倍速設定を6個にして倍速切り替えボタン(Stretch)を追加した
・タイムスタンプの巡回位置を26時間20分後に移動することで、ラップアラウンド時の
  DirectShowの問題を回避できるようにした
・オプション/tvtpofsを追加し、初期再生位置を指定できるようにした
・設定キーMarginを廃止(自動取得に変更)
・設定キーToBottomを廃止(RowPosFullに吸収)
ver.1.0 (2011-10-30)
・コントロールのフォントをTVTestステータスバーのものと合わせるようにした
・再生オフで一時停止するようにした
・再生リストを設けた
・プレイリストファイルの読み込みに対応した
・設定キーButton02のリピート設定ボタンをデフォルトで表示するようにした
・設定キーTsAutoClose/TsAutoLoopを廃止した
  ・今後はTsAutoClose=1(ファイルを再生終了後に閉じる)相当で動作
・ver.1.xの人柱期間中はver.0.9r3のバグにも対応予定
ver.0.9r3 (2011-10-20)
・TVTest ver.0.7.23でボーダーの配色方法が微妙に変化したので追随
・"TVTest.ini"の読み込み部分を0.7.21r2ベースに戻して軽量化+今後のTVH264に対応
・"TvtPlay.ini"のFileInfoセクションの読み込み部分を効率化
・TvtAudioStretchFilterに機能追加
・ver.1.0は人柱版になるかもしれない(ならないかもしれない)
ver.0.9r2 (2011-10-03)
・ドライバ変更時のチャンネル設定の問題が解決されたので、TVTest ver.0.7.22以降に
  ついて、ver.0.8r2での仮対応を元にもどした
・TVTestのINI読み書き方法の変更により、デザインを適用できないことがあるのを修正
・書き込みを参考に簡易一覧表示のキー割り当てを「ファイルを開く(ポップアップ)」の
  みにもどした(もう一回押すと閉じる)
・プラグインの説明(設定->プラグイン->一覧)にバージョンを表記するようにした
ver.0.9 (2011-10-02)
・2sen(hdusup)のup0598を可変ピッチにして流用し、アイコン合成できるようにした
  ・デザインされた方、感謝します
・192ByteTSを188ByteTSに変換して転送できるようにした
・容量確保録画の追っかけ時に総再生時間-3秒に達したら等速再生に戻すようにした
・降順に簡易一覧表示できるようにした
・簡易一覧表示をリモコン操作できるようにした
  ・ちゃんとできるか不明、レスを待ちます
・コントロールのデザインを独自に設定できるようにした
・倍速再生時のフリーズを軽減した
ver.0.8r2 (2011-09-20)
・"倍速再生"の方が正しそうなので用語をあらためた
・容量確保録画の追っかけ再生を考慮した
・フルスクリーン時のマルチディスプレイでの動作、以下2点を修正した
  ・縦サイズが異なるモニタを使っている場合
  ・全画面切り替え時にバーが画面外にいる場合
・複数起動禁止時の起動オプションからのUDP/Pipeへのドライバ変更時、チャンネルセッ
  トするようにした(問題が解決するまでの仮対応)
・簡易一覧表示のポップアップをキー割り当て可能にした
  ・リモコン対応は次バージョン以降
ver.0.8 (2011-09-16)
・早送り機能を追加した
・指定フォルダの簡易一覧表示を追加した
ver.0.7 (2011-09-02)
・移動・リサイズで意味もなくコントロールをアクティブにしてたのを修正
  ・スレで指摘してくれた方、感謝です
・ループ再生時、再生終了後に1秒弱待つようにした
・フルスクリーン時にコントロールを出すとマルチメディアキーが効かなくなるのを修正
・最大化時のコントロールの位置をちょっと工夫した
・放送時刻表示がいまいち見づらかったので書きこみを参考に微調整
・x64版を追加した(Windows7でしかテストしてない)
・設定キーTsThreadPriorityを追加した
・オプション/tvtpudp /tvtpipeを追加した
・簡単なレジューム機能をつけた
ver.0.6 (2011-08-29)
・デッドロックの問題が解決されたので、TVTest ver.0.7.21以降についてビューアリセ
  ットをシーク直前からシーク後にもどした
・ver.0.7.21以降のウインドウ枠に対応、設定キー Margin を追加
・GUIまわりのTVTestからの流用コードについて、ver.0.7.21での修正分をマージ
・基本機能がそろったのでver.0.xの機能追加は停止
  ・おもにバグ修正のみになります
  ・つぎに機能追加するときはver.1.xの人柱版になる予定です
ver.0.5 (2011-08-27)
・対応するTVTestをver.0.7.16に引き上げた(実際にはTvtPlay0.3からこうだった)
・ファイルを開きなおしたときに再生ボタンを再描画していなかったのを修正
・通常表示のときコントロール下部のマージンが1px少なくなってたのを修正
・シーク後にドロップカウントをリセットするようにした
・BonDriver_Fileのソースを参考にPCRの取得部分をより合理的にした
  ・ソースの保管場所書きこんでくれた方サンクスです
・Time Offset Tableをもとに放送時刻を表示可能にした(個人的な需要)
・TVTestに合わせて最終ビルドをVisualC++2005(EE)にして/MT→/MDに変更
ver.0.4 (2011-08-23)
・人柱版表記をはずした(人柱さん達に感謝!)
・ボタンとシーク量、ほか細かい部分をカスタマイズ可能にした
・一時停止後も少しだけ再生が進んでしまうのを改善
・BonDriver_Pipeのパイプのバッファ量を効率的な値に修正
ver.0.3 (2011-08-20)
・スレッドの要望をもとに3点を追加した
  ・複数起動禁止時に複数起動されたとき、ファイルを開きなおすようにした
  ・追っかけ再生時、総再生時間の右端に'+'を付けた
  ・1時間未満の時刻表示を簡略化
・ストリームのアンダーランを抑制する微調整
・一時停止からの復帰時にカクつく場合があったのを修正
・BonDriver_Pipeつくってみた
・マルチディスプレイに対応できてなかったようなので修正(できたかな…)
ver.0.2 (2011-08-16)
・下記の修正では修正できてなかったため、さらに差し替えました
  ・修正分は"diff_02r1_02r2.txt"を参照
ver.0.2 (2011-08-15)
・バグ発見のため15日21時頃に差し替えました
  ・ファイルパス付で起動すると起動時にフリーズする不具合を修正(TVTestの起動完了
    を待たずにRESET_VIEWERを発行したため、TVTest内部でデッドロックした模様)
・スレッドで要望のあったもののうち、とりあえず容易な2点を追加した
  ・プラグインが有効なときはファイルをドラッグ&ドロップできるようにした
  ・キー割り当てを追加した
・コントロールの描画をすこし修正
・マルチディスプレイに対応したかもしれない(環境が無いので…)
ver.0.1 (2011-08-13)
・初版

■ボタンカスタマイズ例
※設定ファイルの該当部分にコピペして利用してください
【ver.1.9のデフォルト】
SeekItemOrder=99
StatusItemOrder=99
IconImage=
SeekA=-60000
SeekB=-30000
SeekC=-15000
SeekD=-5000
SeekE=4000
SeekF=14000
SeekG=29000
SeekH=59000
StretchA=400
StretchB=200
StretchC=50
StretchD=25
Button00=0,Open
Button01=;1,Close
Button02=4:5:14,Loop
Button03=;9,Prev  2,SeekToBgn
Button04='-'6'0,SeekA
Button05=;'-'3'0,SeekB
Button06='-'1'5,SeekC
Button07='-' '5,SeekD
Button08=6,Pause
Button09='+' '5,SeekE
Button10='+'1'5,SeekF
Button11=;'+'3'0,SeekG
Button12='+'6'0,SeekH
Button13=3,SeekToEnd
Button14='*'2'.'0:30~'*'2'.'0,StretchB
Button15='*'0'.'5:30~'*'0'.'5,StretchC
Button16=;'*'1' '.'0:30~'*'4'.'0:30~'*'0'.'2,StretchD,StretchA
Button17=;'*'1' '.'0:30~'*'4'.'0:30~'*'2'.'0:30~'*'0'.'5:30~'*'0'.'2,Width=32,StretchPopup,Stretch

【ボタン数を少なく+チャプター機能も使う】
SeekItemOrder=12
StatusItemOrder=99
IconImage=
SeekA=-60000
SeekB=-30000
SeekC=-15000
SeekD=-5000
SeekE=4000
SeekF=14000
SeekG=29000
SeekH=59000
StretchA=125
StretchB=150
StretchC=200
StretchD=400
StretchE=25
StretchF=50
StretchG=75
Button00=0,Width=12,Open
Button01=;1,Width=12,Close
Button02=4:5:14,Width=12,ListPopup,Loop
Button03=9,Width=12,Prev,SeekToPrev
Button04=31-' '6-20-60,Width=12,SeekA,SeekH
Button05=;31-' '3-20-60,Width=12,SeekB,SeekG
Button06=31-' '1'5-60,Width=12,SeekC,SeekF
Button07=31-' ' ' ' '5' -60,Width=12,SeekD,SeekE
Button08=3,Width=12,SeekToEnd,SeekToNext
Button09='*'1' '.'0:30~'*'1' '.'2:30~'*'1' '.'5:30~'*'2'.'0:30~'*'4'.'0:30~'*'0'.'2:30~'*'0'.'5:30~'*'0'.'7,Width=12,StretchRe,Stretch
Button10=::::::::,Width=0,StretchZ,StretchPopup
Button11=;6,Width=12,Pause
Button12=62,Width=12,AddChapter,RepeatChapter
Button13=64,Width=12,AddChapter,SkipXChapter
Button14=
Button15=
Button16=
Button17=
