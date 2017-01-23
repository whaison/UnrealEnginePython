UnrealEnginePython

Unreal Engine 4にPythonを埋め込む

どのように、なぜ？

これは、Unreal Engine 4（エディタとランタイムの両方）でPython VM全体を埋め込んだプラグインです（バージョン3.x [デフォルトで推奨されているもの]と2.7）。

Python VMは、UE4の内部API +反射システムのすべてに簡単にアクセスしようとします。つまり、プラグインを使用して他のプラグインを作成したり、タスクを自動化したり、ゲームプレイ要素を実装したりすることができます。

それは、青写真やC ++を避ける方法としてではなく、（ゲームをコーディングするのに必要なC ++の量を減らすことは面白いことかもしれませんが）それらの良い仲間としては意味がありません。

もう一つの面白い機能は、プロジェクトがパッケージ化された後でさえ、あなたのPythonコードを変更できるということです。既にパッケージ化されたゲームから完全に新しいゲームを構築する可能性があります。

これに加えて、プラグインはあなたのゲームにPythonを "穏やかに"統合するために、アクタークラス（PyActor）、ポーンクラス（PyPawn）、文字クラス（PyCharacter）、コンポーネントクラス（PythonComponent）を自動的に追加します。

開発メニューでは、 'PythonConsole'にもアクセスできます。これを使用して、Pythonコマンドをエディタから直接トリガすることができます。

公開されているすべてのエンジン機能は、 'unreal_engine'仮想モジュールの下にあります（プラグインに完全にcでコード化されていますので、標準のpythonシェルから 'import unreal_engine'を実行することを期待しないでください）

現在サポートされているUnreal Engineのバージョンは4.12,4.13,4.14です

Windowsでのバイナリインストール（64ビット）

リリースページ（チェックインhttps://github.com/20tab/UnrealEnginePython/releases構成（それ以外の場合のための私達に尋ねる問題を開くには、[あまりにもPythonのバージョンを指定してください]）、ダウンロードに一致するバイナリバージョンがある場合）それ。

バイナリリリースは標準と埋め込みの2つの形式になっています。Standardはあなたのシステムのpythonインストールを使用するので、pythonのインストールディレクトリがあなたのシステムのPATH環境変数にあることを確認してください（そうしないと、プロジェクトの読み込み中にエラーが発生します）。組み込みリリースには組み込みのPythonインストールが含まれているため、システムにPythonをインストールする必要はありません。

プロジェクトルートディレクトリ（同じレベルのContent / .projectsファイル）にプラグインディレクトリを作成し（まだ存在しない場合）、プラグインをそのディレクトリに解凍します。プロジェクトの名前がFooBarの場合は、FooBar / Plugins / UnrealEnginePythonで終了します。

プロジェクトを開き、Edit / Pluginsメニューに移動します。下部に移動し、[プロジェクト/スクリプト言語]でUnrealEnginePythonを有効にします。

プロジェクトを再起動すると、PythonConsoleが "Window / Developer Tools"メニューに表示されます

バイナリリリースは主にエディタスクリプティングに役立ちます。配布用にプロジェクトをパッケージ化し、Pythonランタイムが必要な場合は、ソースリリースが必要です（下記参照）。

代わりに、あなたのpythonせずにプロジェクトをパッケージ化したい場合は、ちょうどこのラインを持っているUnrealEnginePython.upluginを変更してください：https://github.com/20tab/UnrealEnginePython/blob/master/UnrealEnginePython.uplugin#L20はエディタ」として設定"ランタイム"の代わりに "

Windows上のソースからのインストール（64ビット）

現在、python3.6、python3.5、python2.7がサポートされています。Pythonシステム全体のインストール（デフォルトでは公式のpythonディストリビューションはユーザーのホームディレクトリにインストールされています）を含むPATH環境変数を使用することを強くお勧めします（PATH変数を変更した場合は、厳密には必須ではありませんが、PATHが確実に更新されます）。PATH変数にPythonインストールのパスが含まれていない場合、ビルドログ/出力に警告が表示されます。

ソースの公式リリースをダウンロードするか、単にリポジトリを最新のアップデート用に複製してください：

git clone https://github.com/20tab/UnrealEnginePython
デフォルトでは、ビルドプロシージャは、ハードコーディングされた既知のパスを見て、Pythonのインストールを検出しようとします。：あなたが（または自動検出は、単に失敗した）カスタムPythonのインストールを指定したい場合は、この行でソース/ UnrealEnginePython / UnrealEnginePython.Build.csファイルでそれを変更することができますhttps://github.com/20tab/UnrealEnginePython/blob/マスター/ソース/ UnrealEnginePython / UnrealEnginePython.Build.cs＃L10

プラグインをインストールするプロジェクトを選択し、ファイルエクスプローラを開きます（これは叙事詩ランチャーからも実行できます）。

あなたのプロジェクトにプラグイン/ディレクトリ（存在しない場合）を作成し、そのディレクトリにUnrealEnginePythonをコピーします
ファイルエクスプローラから、プロジェクトのメインファイルを右クリックし、「ビジュアルスタジオプロジェクトファイルを生成」を選択します。
プラグイン/ UnrealEnginePythonがソリューションエクスプローラに表示されるはずです
Visual Studioからコンパイルを実行する
コンパイルが終了したら、pythonライブラリをプラグインで見つけることができます（前に説明したシステムのPATHに入っていなければなりません。ビルドしたばかりのプラグインのBinaries / Win64ディレクトリに残酷にコピーする必要があります）
Unreal Engineエディタを再実行することができます
すべてうまくいくと、「ウィンドウ/開発者ツール」メニューに「Python Console」が表示されます

プロジェクトをパッケージ化したい場合は（実行時にPython VMが必要な場合にのみ必要です。ゲームロジックはPythonでプログラミングされています）、Content / Scripts / ue_site.pyファイルがプロジェクト内にあることを確認してください空にする）。ビルド手順の最後に、必要なすべてのPythonスクリプトを最終ディレクトリにコピーしてください。最終ビルドに埋め込みのPythonを追加しない限り、プロジェクトの最終的なユーザーはPythonを自分のシステムにインストールする必要があります。

あなたのpythonなしでパッケージ化したい場合は、ちょうどこのラインを持っているUnrealEnginePython.upluginを変更してください：https://github.com/20tab/UnrealEnginePython/blob/master/UnrealEnginePython.uplugin#L20は、代わりに"の"編集者"として設定ランタイム"

MacOSX上のソースからのインストール

Python.orgから最新の公式のPythonディストリビューションをインストールしてください（インストールは "/Library/Frameworks/Python.framework/Versions/XY"ディレクトリで終了します）。
新しい非現実的なエンジンの空白のC + +プロジェクトを作成します（そうでないとXcodeは初期化されません）
プロジェクトディレクトリにPluginsディレクトリを作成する
Pluginsディレクトリに移動し、プラグインリポジトリをクローンします。
git clone https://github.com/20tab/UnrealEnginePython
エディタを再起動すると、プラグインのビルドの確認を求めるポップアップが表示されます。
プラグインがビルドされたら、出力ログコンソールに行き、 'Python'をフィルタリングします。Python VMバナーが表示されます。
ビルドプロシージャは、自動的にPythonのインストールを検出しようとします。カスタムパスが必要な場合は、ここで編集してください：

https://github.com/20tab/UnrealEnginePython/blob/master/Source/UnrealEnginePython/UnrealEnginePython.Build.cs#L10

MacOSXでのアップグレード

UnrealEnginePythonの最新開発版にアップグレードするには：

プロジェクトディレクトリのPluginsディレクトリに移動し、git pullを使用します。
git pull
プラグインディレクトリからUnrealEnginePython / Binaries / Macに移動する
プラグインライブラリを削除してUnrealEngineにプラグインの再コンパイルを警告する
rm *のは.dylib
エディタを再起動すると、プラグインのビルドの確認を求めるポップアップが表示されます。
プラグインがビルドされたら、出力ログコンソールに行き、 'Python'をフィルタリングします。Python VMバナーが表示されます。
ソースからのインストールLinux（64ビット）

現在推奨されているのは、Ubuntu Xenial（LTS 16.04）64bitです。明らかに、Unreal Engineビルドを既に持っている必要があります（ubuntu xenialでは、エディタを構築するためにclang-3.5パッケージをインストールする必要があります）。python2.7とpython3.5の両方がサポートされており、デフォルトの設定ではpython3が仮定されています（python3-devパッケージを確実にインストールしてください）。

新しいC ++プロジェクトを作成し、プロジェクトが完全に開始されたらエディタを閉じます。
作成したばかりのプロジェクトディレクトリに移動して、プラグインフォルダを作成します
Pluginsフォルダに移動し、プラグインリポジトリをクローンします。
git clone https://github.com/20tab/UnrealEnginePython
プロジェクトを再オープンします。今回は、Pythonプラグインの再ビルドを求めるポップアップが表示されます。はいを選択して待ちます。
注：ターミナルからプロジェクトを実行して、スタートアップログを見ることができます（初めてプラグインをビルドするとき、プラグインをビルドできない場合、関連するログ行を貼り付けるgithubで問題を開くときに便利です）。

python2を使用する場合は、Source / UnrealEnginePython / UnrealEnginePython.Build.csファイルを編集し、pythonHome文字列を "python27"に変更します（python2.7-devパッケージがインストールされていることを確認してください）。

別のPythonインストールを使用する場合は、UnrealEnginePython.Build.csの最後に移動し、includeとlibpythonのパスを適宜変更してください

Linuxでプラグインをアップグレードする

プラグイン/ UnrealEnginePython / Binaries / Linuxの.soファイルを削除し、最新のコードを取得してください。

次回の実行時に、ビルド手順が再開されます。

他のプラットフォームへのインストール

現在のところ、Windows、MacOSX、Linuxだけがサポートされています。私たちは、kivyプロジェクトを通じてAndroidのサポートについても調査しています。

Unreal EngineでのPythonの使用（最終的に）

あなたの目的がエディタをスクリプト化することであるならば、あなたは直接にジャンプすることができます

https://github.com/20tab/UnrealEnginePython/tree/master/docs

と

https://github.com/20tab/UnrealEnginePython/tree/master/examples

最初のディレクトリには特定の分野の公式ドキュメントが含まれていますが、2番目のディレクトリにはプロジェクトの「魔法」を実行するpythonスクリプトのコレクションがあります。

Pythonで管理される新しい青写真クラスの作成

私たちはPython（C ++や青写真の代わりに）に基づいて新しいActorを作成します。

これは「穏やかな」アプローチで、UE4のAPIと対話するための「プロキシ」のPythonクラスを使用しています。あなたがシステムに慣れたら、あなたはさらに行くと柄ネイティブサブクラスのAPIを作業を開始できます（https://github.com/20tab/UnrealEnginePython/blob/master/docs/Subclassing_API.md）

コンテンツブラウザで「新規追加」をクリックし、「青写真クラス」を選択します。

クラスメニューで「PyActor」を選択します：

代替テキスト

ここで新しいアセットが作成され、意味のある名前を付けてダブルクリックすると、青写真エディタでの設定が開始されます

代替テキスト

右側（[詳細]タブ）にはPythonのセクションがあります。

今のところ「Python Module」と「Python Class」のみが意味を持ちます。

プロジェクトのContentディレクトリに移動し、 'Scripts'という名前のディレクトリを作成します。ここにあなたのすべてのPythonモジュールが常駐します。好きなテキストエディタで、新しいpythonモジュール（funnygameclasses.pyなど）を作成し、そこに新しいクラスを定義します：

輸入 unreal_engine として UE

ue.log（'こんにちは私はPythonモジュールです」）

クラス ヒーロー：

    ＃これは、ゲーム開始時に呼び出される
    デフ begin_play（自己）：
        ue.log（'ヒーロークラスにプレイを開始」）

    ＃これは、すべての「チック」で呼ばれている     
    デフ ティック（自己、 DELTA_TIME）：
        ＃は現在の位置の取得 
        場所 =  自己 .uobject.get_actor_location（）
        ＃の DELTA_TIME称える増加Z 
        location.z + =  100  * DELTA_TIME
        ＃設定し、新しい場所の
        自己 .uobjectを.set_actor_location（location）
今、ブループリントエディタに戻り、 'Pythonモジュール'フィールドに 'funnygameclasses'、 'Pythonクラス'フィールドに 'Hero'を設定します。

あなたが見ることができるように、俳優は単にz軸上を移動するだけですが、シーンにフィードバックを付けるためには何らかの視覚的表現を与える必要があります。青写真エディタで「コンポーネントを追加」をクリックし、いくつかのシェイプ（球体、キューブなど）を追加します。青写真を保存してコンパイルします。

今度は、コンテンツブラウザからシーンにbluprintをドラッグして、「再生」をクリックするだけです。

あなたは、あなたの俳優が1メートル/秒の速度で 'z'軸に沿って動くのを見なければなりません

デフォルトでは、 'begin_play'と 'tick'メソッドが必要です（見つかった場合、自動的に考慮されます）。それらに加えて、イベントを定義するための「自動」システムが利用可能である：

デフ on_actor_begin_overlap（自己、私、other_actor）：
     パス

デフ on_actor_end_overlap（自己、私、other_actor）：
     パス

デフ on_actor_hit（自己、私、other_actor、normal_impulse、hit_result）：
     パス

...
基本的に 'on_'で始まる各メソッドに対して、関連するデリゲート/イベントは自動的に設定されます（利用可能な場合）。

代わりに手動でイベントを設定する方が好きなら、次の関数が公開されます：

クラスの ボール：

    デフ begin_play（自己）：
         自己 .uobject.bind_event（' OnActorBeginOverlap 」、自己 .manage_overlap）
         自己 .uobject.bind_action（'ジャンプ'。、UE IE_PRESSED、自己 .uobject.jump）
         自己 .uobject.bind_key（' K '、 UE。IE_PRESSED、自己 .you_pressed_K）
         自己 .uobject.bind_axis（' MoveForward 」、自己 .move_forward）

    デフ manage_overlap（自己、私を、他）：
        ue.print_string（「重複」  + other.get_name（））

    デフ you_pressed_K（自己）：
        （ue.log_warning 「あなたはKを押します」）

     デフ move_forward（自己、量）：
        ue.print_string（'軸値："  +  strの（量））

「self.uobject」とは何ですか？

シームレスなPythonの統合を可能にするために、エンジンの各UObjectは自動的に特別なPythonオブジェクト（ue_PyUObject）にマッピングされます。

あなたがPythonからUObjectにアクセスしたいときはいつでも、ue_PyUObjectへの参照を取得して、そのメソッドを介してUObjectの機能（プロパティ、関数など）を公開します。

この特殊なPythonオブジェクトはメモリ内のC ++マップにキャッシュされます。（キーはUObjectポインタ、値はue_PyUObjectポインタです）

より明確にするために、次の呼び出しを行います。

text_render_component = unreal_engine.find_class（' TextRenderComponent '）
内部的に 'TextRenderComponent'クラスを（非現実的なC ++リフレクションによって）検索し、見つかった場合はキャッシュ内で使用可能かどうかをチェックします。そうでなければ、キャッシュに置かれる新しいue_PyUObjectオブジェクトを作成します。

前の例から 'text_render_component'はUObjectへのマッピングを保持しています（この例ではUClassです）。

注意：PyActor（またはPyPawn、PyCharacter、PyComponent）にマップするPythonクラスは、ue_PyUObjectではありません。これは、関連するue_PyUObjectマップされたオブジェクトへの参照（'uobject 'フィールド経由）を保持する古典的なpythonクラスです。これらのクラスを記述するための最善の技術用語は「プロキシ」です。

これからの 'uobject'についての注意

次の行では、 'uobject'への参照を見つけるたびに、それはue_PyUObjectオブジェクトとして意味されます。

ActorにPythonコンポーネントを追加する

これはPyActorクラスと同じように動作しますが、それはコンポーネントです。あなたはそれを（任意のアクタに「Python」コンポーネントを検索して）添付することができます。

コンポーネントの場合、self.uobjectフィールドはアクタではなくコンポーネント自体を指していることに注意してください。

アクターにアクセスするには、以下を使用します。

俳優=  自己 .uobject.get_owner（）
次の例では、3人目の公式の青写真を非イベントの方法でPythonコンポーネントとして実装しています（これは、非現実的なエンジンでは反パターンです、イベントベースのアプローチについては以下を参照してください）。

輸入 unreal_engine として UE

クラス プレーヤー：
     デフ begin_play（自己）：
        ＃はによりポーン（文字）への参照を取得
        自己 .pawn =  自己）（.uobject.get_ownerを

        ＃は、ポーンが軸が値受け取ることができます
        自己 .pawn.bind_input_axis（ ' TurnRate '）
        自己 .pawn.bind_input_axis（ ' LookUpRateを」）

        ＃次の2つの値は、もともと青写真変数として実装されている
        自己 .base_turn_rate =  45.0 
        自己 .base_look_up_rate =  45.0

         ＃結合する他の軸の
        自己 .pawn.bind_input_axis（「電源を入れ'）
        自己 .pawn.bind_input_axis（「ルックアップ」）

        自己 .pawn.bind_input_axis（' MoveForward '）
         自己 .pawn.bind_input_axis（「 MoveRight 」）

    デフ ティック（自己、DELTA_TIME）：
        ＃ zの回転 
        turn_rate =  自己 .pawn.get_input_axis（' TurnRate '）*  自己 .base_turn_rate * DELTA_TIMEの
         自己 .pawn.add_controller_yaw_input（turn_rate）

        ＃の Y回転 
        look_up_rate =  自己 .pawn.get_input_axis（ ' LookUpRate '） *  自己 .base_look_up_rate * DELTA_TIMEの
        自己 .pawn.add_controller_pitch_input（look_up_rate）

        ＃のマウス垂直
        自己 .pawn.add_controller_yaw_input（自己 .pawn.get_input_axis（「電源を入れ'））

        ＃のマウスの水平
        自己 .pawn.add_controller_pitch_input（自己 .pawn.get_input_axis（「ルックアップ」））

        ＃電流制御回転取得 
        腐敗 =  自己）（.pawn.get_control_rotationを

        ＃は、キャラクタ移動 
        FWD =（ue.get_forward_vectorを 0、 0、腐敗[ 2 ]）
        右= ue.get_right_vector（0、0、rot.yaw）

        自己 .pawn.add_movement_input（FWD、自己 .pawn.get_input_axis（' MoveForward '））
         自己 .pawn.add_movement_input（右、自己 .pawn.get_input_axis（「 MoveRight 」））

        ＃は、ジャンプを管理する
        場合は 自己 .pawn.is_action_pressed（ 'ジャンプ'）：
            自己 .pawn.jump（）
        の場合 、自己 .pawn.is_action_released（ 'ジャンプ'）：
            自己 .pawn.stop_jumping（）
そして、「祝福された」出来事の約束通り、

クラス PlayerEvented：

    デフ begin_play（自己）：
        ＃はによりポーン（文字）への参照を取得
        自己 .pawn =  自己 .uobject.get_ownerを（）

        ＃次の2つの値は、もともと青写真変数として実装された
        自己 .base_turn_rate =  45.0 
        自己 .base_look_up_rate =  45.0

        ＃のバインド軸イベントは、
        自己（.pawn.bind_axis ' TurnRate 」、自己 .turn）
        自己 .pawn.bind_axis（ ' LookUpRate 」、自己 .look_up）
        自己 .pawn.bind_axis（「電源を入れて」、自己 .pawn.add_controller_yaw_input）
        自己を。 pawn.bind_axis（「ルックアップ」、自己 .pawn.add_controller_pitch_input）

        自己 .pawn.bind_axis（' MoveForward 」、自己 .move_forward）
         自己 .pawn.bind_axis（「 MoveRight 」、自己 .move_right）

        ＃のバインドアクション
        自己 .pawn.bind_action（「ジャンプ」、UE。 IE_PRESSED、自己 .pawn.jump）
        自己 .pawn.bind_action（「ジャンプ」、UE。 IE_RELEASED、自己 .pawn.stop_jumping）

    デフ ターン（自己、axis_value）：
        turn_rate = axis_value *  自己 .base_turn_rate *  自己 .uobject.get_world_delta_seconds（）
         自己 .pawn.add_controller_yaw_input（turn_rate）

    デフ look_up（自己、axis_value）：
        look_up_rate = axis_value *  自己 .base_look_up_rate *  自己 .uobject.get_world_delta_seconds（）
         自己 .pawn.add_controller_pitch_input（look_up_rate）

    デフ move_forward（自己、axis_value）：
        腐敗=  自己 .pawn.get_control_rotation（）
        FWD = ue.get_forward_vector（0、0、腐敗[ 2 ]）
         自己 .pawn.add_movement_input（FWD、axis_value）

    デフ move_right（自己、axis_value）：
        腐敗=  自己 .pawn.get_control_rotation（）
        右= ue.get_right_vector（0、0、腐敗[ 2 ]）
         自己 .pawn.add_movement_input（右、axis_value）
ネイティブメソッドVS反射

デフォルトではUObjectクラスが定義GETATTRとSETATTRを非現実的なプロパティと関数のラッパーとして。

つまり、次の呼び出しを行います。

自己 .uobject.bCanBeDamaged =  真
それは

自己 .uobject.set_property（' bCanBeDamaged '、真）
関数呼び出しだけでなく、

VEC =  自己 .uobject.GetActorRightForward（）
手段

VEC =  自己 .uobject.call_function（' GetActorRightForward '）
さらに重要な（そして便利な）K2_関数も自動的に公開されます：

VEC =  自己 .uobject.GetActorLocation（）
次のようになります。

VEC =  自己 .uobject.call_function（' K2_GetActorLocation '）
明らかにメソッド/プロパティを組み合わせることができます：

自己 .uobject.CharacterMovement.MaxWalkSpeed =  600.0
システムが完全な非現実的なAPIの使用を許可しても、リフレクションはネイティブメソッドより遅くなります。

可能であればネイティブメソッドを使用し、ネイティブメソッドとして関数を公開する必要があると思うときはいつでもプルリクエストをオープンしてください。

そう

VEC =  自己 .uobject.get_actor_location（）
方法よりも速いです

VEC =  自己 .uobject.GetActorLocation（）
反射ベースの関数は、キャメルケース（または最初の大文字）の関数です。代わりに、ネイティブ関数は小文字のアンダースコアと区切りの関数名を持つPythonスタイルに従います。

automagic UClassとUEnumsのマッパー

unreal_engine.find_class（name）呼び出しのgazilionを実行する代わりに、プラグインはunreal_engine.classesという 'magic'モジュールを追加します。Pythonクラスのような非現実的なクラスをインポートすることができます：

unreal_engine.classes インポート ActorComponent、ForceFeedbackEffect、KismetSystemLibrary

... 
コンポーネント=  自己 .uobject.get_owner（）。GetComponentsByClass（ActorComponent）

... 
自己 .force_feedback = ue.load_object（ForceFeedbackEffect、' /ゲーム/バイブ」）
 自己 .uobject.get_player_controller（）。ClientPlayForceFeedback（自己 .force_feedback）

... 
名前= KismetSystemLibrary.GetObjectName（自己 .actor）
最後の例は、静的なクラスの関数呼び出しという別の魔法の機能を示しています。明らかに、この特定のケースでは、self.actor.get_name（）を使用するのが最良のアプローチでしたが、この機能により、青写真関数ライブラリにもアクセスできます。

ウィジェットを追加する別の例：

unreal_engine.classes インポート WidgetBlueprintLibraryを

クラス PythonFunnyActor：
     デフ begin_play（自己）：
        WidgetBlueprintLibrary.Create（自己 .uobject、ue.find_class（' velocity_C '））
列挙型、キーワード引数、および出力値を使用するもう1つの複雑な例（出力値は戻り値の後に追加されます）：

輸入 unreal_engine として UE
 から unreal_engine 輸入 FVector、FRotator、FTransform、FHitResult
 から unreal_engine.classesはインポート ActorComponent、ForceFeedbackEffect、KismetSystemLibrary、WidgetBlueprintLibraryを
 から unreal_engine.enums インポート EInputEvent、ETraceTypeQuery、EDrawDebugTraceを

...

is_hitting_something、hit_result = KismetSystemLibrary.LineTraceSingle_NEW（自己 .actor、自己 .actor.get_actor_location（）、FVector（300、300、300）、ETraceTypeQuery.TraceTypeQuery1、DrawDebugType = EDrawDebugTrace.ForOneFrame）
 場合 is_hitting_something：
    ue.log（hit_result）
ue_site.pyファイル

エディタ/エンジンの起動時に、ue_siteモジュールのインポートが試行されます。そこに初期化コードを配置する必要があります。モジュールをインポートできない場合は、ログに（有害な）メッセージが表示されます。

PyPawn

これはPyActorのように機能しますが、今回は新しいPawnクラスを生成します（これはコントローラで可能です）

「世界」コンセプト

すべてのオブジェクトはワールド（UWorld in C ++）にマップされます。一般に、レベルで演奏すると、オブジェクトはすべて同じ世界に住んでいますが、同時に複数の世界が存在する可能性があります（たとえば、エディタでテストするときにエディタの世界とシミュレーションの世界があります）

他の世界を参照することは非常にまれですが、2つのオブジェクトの世界を比較する必要があるかもしれません（例えば、Pythonモジュール内の参照を隠れた世界のオブジェクトに持っていて、 ）。

uobject.get_world（）関数は、ワールドを表すオブジェクト（C ++ UWorldクラス）を返します。

オブジェクトのAPI

各オブジェクトは、エンジンのUObjectクラスを表します。このC ++クラスは、基本的に他のすべてのクラス（アクター、Pawn、コンポーネント、プロパティ...）のルートです。Unreal Engineリフレクションシステムのおかげで、各非現実のエンジンクラスに対してPythonクラスを実装する必要はありませんが、パフォーマンス上の理由から、最も一般的なメソッドが公開されています。uobjectシステムは、マップされたC ++ UObjectの型をチェックし、呼び出すことが安全である場合にのみメソッドを呼び出します。

時には、適切なオブジェクトを自動的に取得するためのメソッドが実装されています。例として、get_actor_location（）はコンポーネント上で呼び出されると自動的に関連するアクターを取得し、C ++のAActor :: GetActorLocation（）メソッドを呼び出します。

このオートマティックアプローチが危険すぎる場合、メソッドはオブジェクトタイプをチェックし、矛盾が発生した場合に例外を発生させます。

リフレクションシステムは、すべての単一エンジンクラスメソッドを実装する必要はないことを覚えておいてください。リフレクションシステムは、プロパティと関数呼び出しによってのみ制御されるほど強力です（uobject call（）メソッドをチェックしてください）

最もよく使用されるメソッドは、パフォーマンス上の理由から、直接、オブジェクトメソッドとして実装されます。

あなたがここにuobject APIメソッドのリストを取得することができますhttps://github.com/20tab/UnrealEnginePython/blob/master/docs/uobject_API.md

自動モジュールリロード（エディターのみ）

エディタでは、プロジェクトを再起動することなく、プロキシにマップされたモジュールのコードを変更できます。PyActor、PyPawn、またはPythonComponentがインスタンス化されるたびに、エディタはモジュールをリロードします。これは明らかに最良の方法ではありません。将来、必要に応じてリロードするために、ファイルにタイムスタンプ監視を実装したいと考えています。

プリミティブと数学関数

プラグインは、FVector、FRotator、FQuat、FColor、FHitResult、および内部ハンドルの束を公開します。

意味のある数学演算が公開されている場所：

輸入 unreal_engine

クラス ActorGoingUp：
     デフ begin_play（自己）：
        ＃秒ずつ1メートル
        自己 .speed =  100

    デフ ティック（自己、DELTA_TIME）：
        ＃がアップベクトル取得 
        アップ=  自己 .uobject.get_up_vector（）
        ＃は、現在位置の取得 
        位置を=  自己 .uobject.get_actor_location（）
        ＃は、速度に基づいて方向ベクトルを構築 
        up_amount =アップ*  自己 .speed * DELTA_TIME）
        ＃の位置に和方向 
        位置+ = up_amount
         ＃は新しい位置の設定
        、自己 .uobject.set_actor_locationを（new_position）
オブジェクトの参照

find_class（）、find_struct（）およびfind_object（）関数を使用して、すでにロードされているクラス/オブジェクトを参照することができます。

エンジンにロードされていないアセット（まだ）を参照する必要がある場合は、load_struct（）、load_class（）またはload_object（）を使用できます。

a_struct_data = ue.load_struct（' /ゲーム/データ」）
ue.log（a_struct_data.as_dict（））
特定の資産を見つけることができます：

texture_class = ue.find_class（'にTexture2D '）
a_specific_texture = ue.load_object（texture_class、' /ゲーム/テクスチャ/ロゴ2 」）
as_dict（）メソッド

この特別なメソッドは任意のuobjectで呼び出すことができます：それはPython辞書にそれをシリアル化しようとします

ナビゲーション

唯一公開されているナビゲーション関連のメソッドは 'simple_move_to_location'です。それは、（キャラクターのような）動き成分を持つPawnを期待しています。

クラス MoveToTargetComponent：

    デフ begin_play（自己）：
        ＃ポーンパブリックプロパティから「目標点」参照を取得し 
        、ターゲット=  自己。）（.uobject.get_ownerをGET_PROPERTY（'ターゲット'）
         自己。）.uobject.get_ownerを（simple_move_to_location（target.get_actor_location（） ）

    デフ ティック（自己、DELTA_TIME）：
         パスを
ディアブロのような動きの別の例（クリックして移動する、このコンポーネントを文字に追加する）

クラス ウォーカー：
     デフ begin_play（自己）：
         自己 .uobject.show_mouse_cursor（）
     デフ ティック（自己、DELTA_TIME）：
         場合 ない 自己 .uobject.is_input_key_down（' LeftMouseButton '）：
             リターン 
        ヒット=  自己 .uobject.get_hit_result_under_cursor（UE。COLLISION_CHANNEL_VISIBILITY）
         場合 ではないヒット：
             リターン
        ＃のヒットがunreal_engine.FHitResultオブジェクトである
        自己 .uobject.simple_move_to_location（hit.impact_point）
物理

'set_simulate_physics'メソッドは、PrimitiveComponentの物理を有効にするために公開されています。

衝突のプリセットを適切に設定しなくても、物理学を有効にすることはできません。

＃の staticmeshcomponent（球）とPyActor 
＃それは下剤のオブジェクトになっ重なっ
クラスの ボール：

    デフ begin_play（自己）：
         自己 .sphere =  自己 .uobject.get_actor_component_by_type（ue.find_class（' StaticMeshComponent '））

    デフ ティック（自己、DELTA_TIME）：
         パスを

    デフ on_actor_begin_overlap（自己、私は、other_actor）：
        ＃の変更衝突プロファイル
        自己 .sphere.call（' SetCollisionProfileName BlockAll '）
         ＃は、物理学の有効
        自己）（.sphere.set_simulate_physicsを

    ＃オブジェクトがイベントをヒット、物理一つとなった後には到着し始めます
    デフ on_actor_hitを（自己、私、他の、 * 引数）：
        （ue.print_string 'でHIT 」  + other.get_name（））
TODO：より多くの物理機能を公開します。特に、力を加えるためのものです。

タイマー

カスタムPythonクラスが公開され（unreal_engine.ftimerHandler）、UE4タイマーをサポートします：

＃タイマーの作成 
タイマー =  自己 .uobject.set_timer（周波数を、呼び出し可能な [、ループは、初期]）
＃は、タイマをクリア
timer.clear（）
＃タイマーを一時停止
timer.pause（）
＃タイマーの一時停止を解除 
）（timer.unpauseを
骨折

フラクチャリングは、あなたが非現実のエンジンで無料で入手できる最高の機能の1つです。

あなたはPythonから直接破壊可能なオブジェクトにダメージを与えることができます（もっと多くの種類のダメージがすぐに追加されます）

クラス DestroyMeComponent：

    デフ begin_play（自己）：
        ＃は俳優の中で破壊コンポーネントへの参照を取得
        自己 .destructible =  自己 .uobject.get_actor_component_by_type（ue.find_classを（' DestructibleComponent '））

    デフ ティック（自己、DELTA_TIME）：
         パスを

    デフ 爆発（自己）：
        ＃のダメージ量の 
        量=  1000年 
        強度=  20000 
        位置=  自己 .uobject.get_ownerを（）get_actor_locationを（）。
        アップ=  自己 .uobject.get_owner（）。get_actor_up（）
         自己（最大量、強度、位置、）.destructible.destructible_apply_damage
'Call Python Component Method'ノードを使用して青写真を介して 'explode'メソッドを呼び出すことができます

別のアプローチ（より簡単な方法）では、RadialForceComponentを追加して、何かを破壊したいときに起動します：

＃ RadialForceComponentのへの参照を取得
自己 .radial_forceを =  自己 .uobject.get_owner（）。get_actor_component_by_type（ue.find_class（ ' RadialForceComponent '））

＃それを発射！
自己 .radial_force.call（ ' FireImpulse '）
スプライン

スプラインはUE4の素晴らしい機能です。

次のコンポーネントは、スプラインパスを介して別のアクタを移動する方法を示しています。

クラス スプライン：
     デフ begin_play（自己）：
        ＃は、スプラインコンポーネントへの参照を取得
        自己 .spline =  自己）（.uobject.get_ownerをget_actor_component_by_type（ue.find_class（。' SplineComponent '））
        ＃は、スプラインの長さを見つける
        自己 .max_distanceを=  自己 .spline.get_spline_length（）
         自己 .distanceは=  0.0 
        ＃は（青写真プロパティとして）移動するアクターへの参照を取得し
        、自己 .actor_to_move =  自己 .uobject.get_ownerを（）。GET_PROPERTY（' ObjectToMove '）

    デフ ティック（自己、DELTA_TIME）：
         場合は 自己 .distance > =  自己 .max_distance：
             リターン
        ＃はスプライン上の次のポイントを見つける 
        next_point =  自己 .spline.get_world_location_at_distance_along_spline（自己 .distance）
         自己 .actor_to_move.set_actor_location（next_point）
         自己 .distanceを+ =  100  * DELTA_TIME
Blueprintsの統合

.call（）および.call_function（）メソッドを使用して、青写真関数（またはカスタムイベント）を呼び出すことができます。

your_funny_blueprint_object.call（' AFunctionOrACustomEvent with_a_arg 」）
外部オブジェクトを参照する必要があるときはいつでも、find_object（）などを使用しないでください。代わりに、特定のオブジェクトを指しているパブリック変数を青写真に追加します。次に、このオブジェクトを簡単に参照してプロパティ値を取得することができます。

the_other_object =  自己 .uobject.get_property（「ターゲット」）
the_other_object.set_actor_location（0、0、0）
.call_function（）はより高度です。戻り値とPythonの引数を考慮しています：

＃曲線を持つオブジェクトzを移動する例：
クラス Curver：
    デフ begin_play（自己）：
        自己 .curve =  自己。.uobject.get_owner（）GET_PROPERTY（ '曲線'）
        自己 .accumulator =  0.0 
    デフ ティック（自己、 DELTA_TIMEを） ：
        場所=  自己 .uobject.get_actor_location（）
        Z =  自己 .curve.call_function（' GetFloatValue 」、自己 .accumulator）*  100 
        自己 .uobject.set_actor_location（location.x、location.y、z）の
         自己 .accumulator + = DELTA_TIME
イベント

イベント（以前のように）をbind_event関数で簡単にバインドすることができます

自己 .uobject.bind_event（' OnActorBeginOverlap 」、a_funny_callback）
Pythonから直接カスタムイベントをマッピングするためのサポートが現在行われています。

青写真からPython関数にイベントをマップする場合は、さまざまなプラグインクラスによって公開されている「python call」青写真関数を使用するのが最も良い方法です。

代替テキスト

オーディオ

uobject.play_sound_at_location（sound、position [、volume_multiplier、pitch_multiplier、start_time]）apiメソッドが公開されています：

＃ asoundへの参照を取得する 
音 = ue.find_object（ ' my_soundを'）
＃は、位置で0,0,0サウンドを再生
自己 .uobject.play_sound_at_location（音、FVector（ 0、 0、 0））
AudioComponentを使いたい場合：

クラス サウンダ：
     デフ begin_play（自己）：
        ＃は、この俳優のAudioComponent見つける
        自己 .AUDIO =  自己 .uobject.get_component_by_type（' AudioComponent '）
         自己 .audio.call（「ストップ」）
     デフ ティック（自己、DELTA_TIMEを）：
        ＃の開始押す音'' 
        の場合 、自己 .uobject.is_input_key_down（' A '）：
             自己 .audio.call（'プレイ'）
アニメーション

アニメーションの青写真の変数やイベントを簡単に制御できます：

＃骨格メッシュへの参照を取得 
骨格 =  自己 .uobject.get_component_by_type（ ' SkeletalMeshComponent '）
＃をアニメーションクラスへの参照を取得 
アニメーション =）（skeletal.get_anim_instanceを

＃は変数設定 
animation.set_property（「スピード」、 17.0）

＃カスタムイベントトリガー 
animation.call（ ' AttackWithSwordを」）
パッケージング

プロジェクトをパッケージ化するときは、バイナリフォルダとScriptsディレクトリにlibpython（dllまたはdylib）を含めてください。

あなたがPythonのソースを配布したくない場合は、あなただけ含めることができます__pycache__バイトコードでディレクトリを。

Pythonのサードパーティ製モジュールを含めることを忘れないでください（あなたのプロジェクトでそれらのモジュールを使用している場合）

例

これは、別のアクターが重複しているときには、それ自体を破壊するPyActorです。メッシュコンポーネントを（球のように）追加し、その衝突動作を 'OverlapAll'として設定することを忘れないでください。これは、第三者の公式テンプレートでテストすることができます。

クラスの ボール：
     デフ begin_play（自己）：
        （ue.print_string 'こんにちは'）
     デフ ティック（自己、DELTA_TIME）：
         渡す
    デフ on_actor_begin_overlap（自己を、other_actor）：
        ue.print_string（'と衝突」  + other_actor.get_name（））
         自己 .uobject.actor_destroy（）
今度はPyActorを完全に（実行時に!!!）作成します：

クラスの スーパーヒーロー：
     デフ begin_play（自己）：
         ＃1スポーン新しいPyActor 
        new_actor =  自己 .uobject.actor_spawn（ue.find_class（' PyActor '）、Fvector（0、0、0）、FRotator（0、0、90））
        ＃をルート1のように球のコンポーネントを追加 
        static_mesh =（ue.find_class（new_actor.add_actor_root_component ' StaticMeshComponent '、）' SphereMesh '）
         ＃は、球の資産としてメッシュを設定 
        static_mesh.call（' SetStaticMesh /Engine/EngineMeshes/Sphere.Sphere 」）
         ＃は、 Pythonのモジュール設定 
        new_actor.set_property（' PythonModule '、' gameclasses '）
         ＃は、 Pythonのクラス設定 
        new_actor.set_property（' PythonClass 」、「垂直に」）

    デフ ティック（自己、DELTA_TIME）：
         パスを
PyQTとの統合

PyQTをUnrealEngineと正しく統合するには、pythonプラグインが正しくGILをセットアップしなければならない（これが行われます）例外はアドホックで管理する必要があります（qtシグナルハンドラが例外を発生させるとデッドロックが発生します）

これは、エディタに沿ってアセットの再インポートを開始するQTウィンドウを持つ例です（sys.excepthookの使用に注意してください）。

PyQt5.QtWidgets インポートはQApplication、QWidgetの、QListWidgetの
 輸入 unreal_engineをとして UE
 輸入のsys
 輸入トレースバック

デフ ue_exception（_type、値、バック）：
    ue.log_error（値）
    tb_lines = traceback.format_exception（_type、値、バック）
     のためのラインで tb_lines：
        ue.log_error（行）

sys.excepthook = ue_exception

skeletal_mappings = {}

デフ selected_skeletal_mesh（アイテム）：
    uobject = skeletal_mappings [item.data（）]
    ue.log（「再インポートする準備ができました：'  + uobject.get_name（））
    uobject.asset_reimport（）

アプリ=はQApplication（[]）

勝利=はQWidget（）
win.setWindowTitle（「アンリアル・エンジン4骨格メッシュのreimporter '）

wlist = QListWidget（勝利）
 のための資産で ue.get_assets_by_class（「骨格メッシュ'）：
    wlist.addItem（asset.get_name（））
    skeletal_mappings [asset.get_name（）] =資産

wlist.clicked.connect（selected_skeletal_mesh）
wlist.show（）

win.show（）
メモリ管理

2種類のGCを扱うのは本当に難しい作業です。

PyActor、PyPawn、およびPythonComponentは、自動的にDECREFにマップされたクラスです。これは、あなたがPythonモジュール自体で参照を保持しない限り、十分であるはずです。これはプログラミングの習慣が悪いので、現在のアプローチは十分安全でなければなりません。

これに加えて、uObjectはUObjectマッピングを返さなければならないたびに、その妥当性と安全性をチェックします。

＃が定義 ue_py_check（py_u）の場合（！py_u-> ue_object ||！py_u-> ue_object-> IsValidLowLevel（）|| py_u-> ue_object-> IsPendingKillOrUnreachable（））\
                            （、PyExc_Exception PyErr_Formatを返す」 PyUObjectが無効な状態にあります」）
ユニットテスト

リポジトリには、TestingActorアセットとtests.pyスクリプトが含まれています。アセットとスクリプトをプロジェクトに追加するだけで、テストスイートが実行されます。テストスイートはまだまだ改善される試作品です。

スレッディング（実験的）

デフォルトでは、プラグインは効果的なPythonスレッドのサポートなしでコンパイルされます。これは主に2つの理由によるものです。

GILを常に取得してリリースすることによるパフォーマンスの影響については、まだ数字がありません
私たちはより良いテストスイートが必要です
ちなみに、実験的なスレッドのサポートをしたい場合は、コメントを外してください

//の#define UEPY_THREADING 1
UnrealEnginePythonPrivatePCH.hの上に配置し、プラグインを再構築します。

ネイティブスレッドと同様に、非メインスレッドからのUObjectを変更（インクルードされた削除）しないでください。

UObjectからのPythonプロキシへのアクセス

ときには、UObjectを持っていて、それがPythonオブジェクトに裏打ちされていることを知っていることがあります。UObjectからPythonオブジェクトを取得するには、使用get_py_proxy方法を。たとえば、次のような状況があるとします。

あるPyActorというサブクラスPyExplosiveActorがありExplosive、そのPythonのクラスとしては。
Explosive持っているgo_boomのpython方法を。
あるPyActorというサブクラスPyBadGuyActorと呼ばれる青写真プロパティがありMyBombと呼ばれるPythonのクラスがBadGuy。
BadGuypythonでのインスタンスは、そのUObjectはそのを持っていることを知っているMyBombのインスタンスとしてPyExplosiveActorとコールしたいgo_boomのpython方法を。
これは以下のように解決されます。

import unreal_engine as ue

class Explosive:
    'Python representation for PyExplosiveActor in UE4'

    def go_boom(self):
        # do python stuff to explode
        ...
        self.uobject.destory()

class BadGuy:
    'Python reprsentation for PyBadGuyActor in UE4'

    def ignite_bomb(self, delay):
        bomb = self.uobject.MyBomb
        py_bomb = bomb.get_py_proxy()
        py_bomb.go_boom()

何でここで起こっていることBadGuyself.uobjectがPyActor UObjectへの参照であるとということですself.uobject.MyBombへの参照ですPyExplosiveuobject。しかし、その代わりに、あなたは（そのプロキシクラスにアクセスしたいですExplosive）。get_py_proxy()この方法は、Pythonのカスタムクラスを返すExplosiveことをPyExplosiveActor、オブジェクトがマップされています。

ステータスと既知の問題

プロジェクトはベータ状態であると考えることができます。

完全なue4 apiを公開することは膨大な量の作業であり、あなたの特定のニーズに応じて自由にリクエストを行うことができます。

.pyファイルはエディタで認識されません。これは間もなく修正されるはずです

まだプラグインアイコンはありません。

ビルドシステムはそれほど堅牢ではありません。たぶんPythonの静的ライブラリをプラグインDLLにリンクする方が良い方法かもしれません。

連絡先と商用サポート

私たちに連絡（ヘルプ、サポート、スポンサーシップ）したい場合は、20tab.comの情報にメールをドロップするか、Twitterで@unbitに従ってください

UnrealEngineとUnrealEnginePythonの商用サポートを提供し、さらに20tab.comの情報にメールを送ってより多くの情報を入手します








-------------------------------------Eng---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------













# UnrealEnginePython
Embed Python in Unreal Engine 4

# How and Why ?

This is a plugin embedding a whole Python VM (versions 3.x [the default and suggested one] and 2.7) In Unreal Engine 4 (both the editor and runtime).

The Python VM tries to give easy access to all of the UE4 internal api + its reflection system. This means you can use the plugin to write other plugins, to automate tasks and to implement gameplay elements.

It is not meant as a way to avoid blueprints or c++ but as a good companion to them (albeit reducing the amount of c++ required for coding a game could be an interesting thing ;)

Another funny feature is that you can change your python code even after the project has been packaged. You can potentially build a completely new game from an already packaged one.

In addition to this, the plugin automatically adds an actor class (PyActor), a pawn class (PyPawn), a character class (PyCharacter) and a component class (PythonComponent) for "gentle" integration of python in your games.

In the development menu, you get access to the 'PythonConsole' too, you can use it to trigger python commands directly from the editor.

All of the exposed engine features are under the 'unreal_engine' virtual module (it is completely coded in c into the plugin, so do not expect to run 'import unreal_engine' from a standard python shell)

The currently supported Unreal Engine versions are 4.12, 4.13 and 4.14

# Binary installation on Windows (64 bit)

Check in the releases page (https://github.com/20tab/UnrealEnginePython/releases) if there is a binary version that matches your configuration (otherwise open an issue asking us for it [please specify the python version too]) and download it.

Binary releases are in two forms: standard and embedded. Standard uses the python installation of your system, so ensure the python installation directory is in your system PATH environment variable (otherwise you will get an error while loading your project). Embedded releases include an embedded python installation so you do not need to have python in your system.

Create (if it does not already exist) a Plugins directory in your project root directory (at the same level of Content/ and the .uproject file) and unzip the plugin into it. If your project is named FooBar you will end with FooBar/Plugins/UnrealEnginePython.

Open your project and go to the Edit/Plugins menu. Go to the bottom and under "Project/Scripting Languages" enable UnrealEnginePython.

Restart your project and you should see the PythonConsole under the "Window/Developer Tools" menu

Binary releases are mainly useful for editor scripting, if you want to package your project for distribution and you need the python runtime, you need a source release (see below).

If instead, you want to package your project without python, just remember to change the UnrealEnginePython.uplugin to have this line: https://github.com/20tab/UnrealEnginePython/blob/master/UnrealEnginePython.uplugin#L20 set as "Editor" instead of "Runtime"

# Installation from sources on Windows (64 bit)

Currently python3.6, python3.5 and python2.7 are supported. It is highly suggested to have a python system wide installation (by default the official python distributions are installed in user's home directory) with the PATH environment variable including it (if you change the PATH variable remember to reboot the system before running the build procedure, this is not strictly required but will ensure the PATH is updated). If the PATH variable does not contain the path of your python installation you will see a warning in the build log/output.

Download a source official release or simply clone the repository for latest updates:

```sh
git clone https://github.com/20tab/UnrealEnginePython
```

By default the build procedure will try to discover your python installation looking at hardcoded known paths. If you want to specify a custom python installation (or the autodetection simply fails) you can change it in the Source/UnrealEnginePython/UnrealEnginePython.Build.cs file at this line: https://github.com/20tab/UnrealEnginePython/blob/master/Source/UnrealEnginePython/UnrealEnginePython.Build.cs#L10


choose a project you want to install the plugin into, open the file explorer (you can do it from the epic launcher too) and:

* create a Plugins/ directory (if it does not exist) in your project and copy the directory UnrealEnginePython into it
* from the file explorer right click on the project main file and choose 'generate visual studio project files'
* open visual studio, you should now see Plugins/UnrealEnginePython in your solution explorer
* run the compilation from visual studio
* once the compilation ends, double check the python libraries can be found by the plugin (they must be in the system PATH like previously described, or brutally copy them in the Binaries/Win64 directory of the just built plugin)
* now you can re-run the unreal engine editor

If all goes well, you will see 'Python Console' in the "Window/Developer Tools" menu

If you want to package your project (it is required only if you need to have a python VM at runtime, read: your game logic is programmed in python) ensure the Content/Scripts/ue_site.py file is in your project (it can be empty). At the end of the build procedure ensure to copy all of your required python scripts in the final directory. Remember that unless you add an embedded python in your final build, the final users of your project will require python installed in his/her system.

If you want to package without python, just remember to change the UnrealEnginePython.uplugin to have this line: https://github.com/20tab/UnrealEnginePython/blob/master/UnrealEnginePython.uplugin#L20 set as "Editor" instead of "Runtime"

# Installation from sources on MacOSX

* install the latest official python distribution from python.org (the installation will end in the "/Library/Frameworks/Python.framework/Versions/X.Y" directory).
* create a new unreal engine blank c++ project (NOT a blueprint one, otherwise XCode will not be initialized)
* create a Plugins directory in the project directory
* move to the Plugins directory and clone the plugin repository


```sh
git clone https://github.com/20tab/UnrealEnginePython
```

* restart the editor and a popup should appear asking your for confirmation of the build of the plugin.
* Once the plugin is built, go to the output log console and filter for 'Python'. You should see the Python VM banner.

The build procedure will try to automatically discover python installations. If you need custom paths, just edit here:

https://github.com/20tab/UnrealEnginePython/blob/master/Source/UnrealEnginePython/UnrealEnginePython.Build.cs#L10

Upgrading on MacOSX
-------------------

To upgrade to the latest development version of UnrealEnginePython:
* move to the Plugins directory in the project directory and use git pull

```sh
git pull
```

* move to UnrealEnginePython/Binaries/Mac from the Plugin directory
* remove the plugin libraries to warn UnrealEngine to recompile the plugin

```sh
rm *.dylib
```

* restart the editor and a popup should appear asking your for confirmation of the build of the plugin.
* Once the plugin is built, go to the output log console and filter for 'Python'. You should see the Python VM banner.

Installation from sources On Linux (64 bit)
-------------------------------------------

Currently the suggested distribution is Ubuntu Xenial (LTS 16.04) 64bit. Obviously you need to already have an Unreal Engine build (note that on ubuntu xenial you need to install the clang-3.5 package to build the editor). Both python2.7 and python3.5 are supported and the default configuration assumes python3 (so ensure to install the python3-dev package).

* Create a new C++ project and close the editor once the project is fully started
* go to the just created project directory and create the Plugins folder
* move to the Plugins folder and clone the plugin repository:


```sh
git clone https://github.com/20tab/UnrealEnginePython
```

* re-open your project, this time you will get a popup asking you for re-building the python plugin. Choose yes and wait.

NOTE: always run your project from a terminal so you can see startup logs (they are really useful when building the plugin the first time, if you cannot build the plugin, open an issue on github pasting the related log lines).

If you want to use python2 just edit the Source/UnrealEnginePython/UnrealEnginePython.Build.cs file and change the pythonHome string to "python27" (ensure to have the python2.7-dev package installed).

If you want to use an alternative python installation, go to the end of UnrealEnginePython.Build.cs, and change the paths of includes and libpython accordingly

Upgrade the plugin on Linux
---------------------------

Just remove the .so files in Plugins/UnrealEnginePython/Binaries/Linux and pull the latest code.

At the next run the build procedure wil be started again.

# Installation on other platforms

Currently only Windows, MacOSX and Linux are supported. We are investigating Android support too via the kivy project.

# Using Python with Unreal Engine (finally)

If your objective is to script the editor, you can directly jump to

https://github.com/20tab/UnrealEnginePython/tree/master/docs

and

https://github.com/20tab/UnrealEnginePython/tree/master/examples

The first directory contains the official documentation for specific areas, while the second one is a collection of python scripts doing any sort of 'magic' with your project ;)

Creating a new blueprint class managed by python
------------------------------------------------

We are going to create a new Actor based on python (instead of C++ or blueprints)

This is the "gentle" approach, using a 'proxy' python class to speak with the UE4 api. Once you get familiar with the system, you can
go further and start working withe native subclassing api (https://github.com/20tab/UnrealEnginePython/blob/master/docs/Subclassing_API.md) 

In the content browser click on 'add new' and choose 'blueprint class'

In the classes menu choose 'PyActor':

![Alt text](screenshots/unreal_screenshot1.png?raw=true "Screenshot 1")

You now have a new asset, give it a meaningful name, and double click on it to start configuring it in the blueprint editor

![Alt text](screenshots/unreal_screenshot2.png?raw=true "Screenshot 2")

On the right (in the 'Details' tab) you will find the Python section.

For now only 'Python Module' and 'Python Class' are meaningful.

Go to the Content directory of your project and create a directory named 'Scripts'. This is where all of your python modules will reside. With your favourite text editor create a new python module (like funnygameclasses.py), and define a new class into it:

```py
import unreal_engine as ue

ue.log('Hello i am a Python module')

class Hero:

    # this is called on game start
    def begin_play(self):
        ue.log('Begin Play on Hero class')
        
    # this is called at every 'tick'    
    def tick(self, delta_time):
        # get current location
        location = self.uobject.get_actor_location()
        # increase Z honouring delta_time
        location.z += 100 * delta_time
        # set new location
        self.uobject.set_actor_location(location)

```

Now, go back to the blueprint editor and set 'funnygameclasses' in the 'Python Module' field, and 'Hero' in 'Python Class'

As you can see the actor will simply move over the z axis, but we need to give it some kind of visual representation to have a feedback in the scene. In the blueprint editor click on 'add component' and add some shape (a sphere, or a cube, or whatever you want). Save and Compile your blueprint.

Now you can drag the bluprint from the content browser to the scene and just click 'Play'.

You should see your actor moving along the 'z' axis at a speed of 1 meter per second

By default a 'begin_play' and a 'tick' method are expected (they will be automatically taken into account if found). In addition to them an 'automagic' system for defining event is available:

```py
def on_actor_begin_overlap(self, me, other_actor):
    pass

def on_actor_end_overlap(self, me, other_actor):
    pass
    
def on_actor_hit(self, me, other_actor, normal_impulse, hit_result):
    pass

...
```

Basically for each method startwing with 'on_' the related delegate/event is automatically configured (if available).

If you instead prefer to manually setup events, the following functions are exposed:

```py

class Ball:

    def begin_play(self):
        self.uobject.bind_event('OnActorBeginOverlap', self.manage_overlap)
        self.uobject.bind_action('Jump', ue.IE_PRESSED, self.uobject.jump)
        self.uobject.bind_key('K', ue.IE_PRESSED, self.you_pressed_K)
        self.uobject.bind_axis('MoveForward', self.move_forward)
        
    def manage_overlap(self, me, other):
        ue.print_string('overlapping ' + other.get_name())
        
    def you_pressed_K(self):
        ue.log_warning('you pressed K')
        
     def move_forward(self, amount):
        ue.print_string('axis value: ' + str(amount))
        

```


What is 'self.uobject' ?
------------------------

To allow seamless Python integration, each UObject of the engine is automatically mapped to a special Python Object (ue_PyUObject).

Whenever you want to access a UObject from python, you effectively get a reference to a ue_PyUObject exposing (via its methods) the features of the UObject (properties, functions, ....)

This special python object is cached into a c++ map in memory. (The key is the UObject pointer, the value is the ue_PyUObject pointer)

To be more clear, a call to:

```py
text_render_component = unreal_engine.find_class('TextRenderComponent')
```

will internally search for the 'TextRenderComponent' class (via unreal c++ reflection) and when found will check if it is available in the cache, otherwise it will create a new ue_PyUObject object that will be placed in the cache.

From the previous example the 'text_render_component' maintains a mapping to the UObject (well a UClass in this example).

Pay attention: the python class you map to the PyActor (or PyPawn, PyCharacter or PyComponent), is not a ue_PyUObject. It is a classic python class that holds a reference (via the 'uobject' field) to the related ue_PyUObject mapped object. The best technical term to describe those classes is 'proxy'.

Note about 'uobject' from now on
---------------------------------

In the following lines, whenever you find a reference to 'uobject' it is meant as a ue_PyUObject object.

Adding a python component to an Actor
-------------------------------------

This works in the same way as the PyActor class, but it is, well, a component. You can attach it (search for the 'Python' component) to any actor.

Remember that for components, the self.uobject field point to the component itself, not the actor.

To access the actor you can use:

```py
actor = self.uobject.get_owner()
```

The following example implements the third person official blueprint as a python component in non-evented way (this is an anti-pattern in unreal engine, see below for the evented-based approach):

```py
import unreal_engine as ue

class Player:
    def begin_play(self):
        # get a reference to the owing pawn (a character)
        self.pawn = self.uobject.get_owner()
        
        # allows the pawn to receive axis values
        self.pawn.bind_input_axis('TurnRate')
        self.pawn.bind_input_axis('LookUpRate')
        
        # the following two values are originally implemented as blueprint variable
        self.base_turn_rate = 45.0
        self.base_look_up_rate = 45.0
        
         # bind the other axis
        self.pawn.bind_input_axis('Turn')
        self.pawn.bind_input_axis('LookUp')

        self.pawn.bind_input_axis('MoveForward')
        self.pawn.bind_input_axis('MoveRight')
        
    def tick(self, delta_time):
        # z rotation
        turn_rate = self.pawn.get_input_axis('TurnRate') * self.base_turn_rate * delta_time
        self.pawn.add_controller_yaw_input(turn_rate)

        # y rotation
        look_up_rate = self.pawn.get_input_axis('LookUpRate') * self.base_look_up_rate * delta_time
        self.pawn.add_controller_pitch_input(look_up_rate)

        # mouse vertical
        self.pawn.add_controller_yaw_input(self.pawn.get_input_axis('Turn'))

        # mouse horizontal
        self.pawn.add_controller_pitch_input(self.pawn.get_input_axis('LookUp'))

        # get current control rotation
        rot = self.pawn.get_control_rotation()
        
        # move the character
        fwd = ue.get_forward_vector(0, 0, rot[2])
        right = ue.get_right_vector(0, 0, rot.yaw)

        self.pawn.add_movement_input(fwd, self.pawn.get_input_axis('MoveForward'))
        self.pawn.add_movement_input(right, self.pawn.get_input_axis('MoveRight'))

        # manage jump
        if self.pawn.is_action_pressed('Jump'):
            self.pawn.jump()
        if self.pawn.is_action_released('Jump'):
            self.pawn.stop_jumping()
```

And as promised the 'blessed' evented approach:

```py
class PlayerEvented:
    
    def begin_play(self):
        # get a reference to the owing pawn (a character)
        self.pawn = self.uobject.get_owner()

        # the following two values were originally implemented as blueprint variable
        self.base_turn_rate = 45.0
        self.base_look_up_rate = 45.0

        # bind axis events
        self.pawn.bind_axis('TurnRate', self.turn)
        self.pawn.bind_axis('LookUpRate', self.look_up)
        self.pawn.bind_axis('Turn', self.pawn.add_controller_yaw_input)
        self.pawn.bind_axis('LookUp', self.pawn.add_controller_pitch_input)

        self.pawn.bind_axis('MoveForward', self.move_forward)
        self.pawn.bind_axis('MoveRight', self.move_right)

        # bind actions
        self.pawn.bind_action('Jump', ue.IE_PRESSED, self.pawn.jump)
        self.pawn.bind_action('Jump', ue.IE_RELEASED, self.pawn.stop_jumping)

    def turn(self, axis_value):
        turn_rate = axis_value * self.base_turn_rate * self.uobject.get_world_delta_seconds()
        self.pawn.add_controller_yaw_input(turn_rate)

    def look_up(self, axis_value):
        look_up_rate = axis_value * self.base_look_up_rate * self.uobject.get_world_delta_seconds()
        self.pawn.add_controller_pitch_input(look_up_rate)

    def move_forward(self, axis_value):
        rot = self.pawn.get_control_rotation()
        fwd = ue.get_forward_vector(0, 0, rot[2])
        self.pawn.add_movement_input(fwd, axis_value)

    def move_right(self, axis_value):
        rot = self.pawn.get_control_rotation()
        right = ue.get_right_vector(0, 0, rot[2])
        self.pawn.add_movement_input(right, axis_value)
```

Native methods VS reflection
----------------------------

By default the UObject class defines __getattr__ and __setattr__ as wrappers for unreal properties and functions.

This means that calling:

```py
self.uobject.bCanBeDamaged = True
```

it is the same as

```py
self.uobject.set_property('bCanBeDamaged', True)
```

As well as function calls:

```py
vec = self.uobject.GetActorRightForward()
```

means

```py
vec = self.uobject.call_function('GetActorRightForward')
```

And more important (and handy) K2_ functions are automagically exposed too:

```py
vec = self.uobject.GetActorLocation()
```

is equal to:

```py
vec = self.uobject.call_function('K2_GetActorLocation')
```

Obviously you can combine methods/properties:

```py
self.uobject.CharacterMovement.MaxWalkSpeed = 600.0
```

Albeit the system allows for full unreal api usage, reflection is slower than native methods.

Try to use native methods whenever possible, and open pull request whenever you think a function should be exposed as native methods.

So

```py
vec = self.uobject.get_actor_location()
```

is way faster than

```py
vec = self.uobject.GetActorLocation()
```

Reflection based functions are those in camelcase (or with the first capital letter). Native functions instead follow the python style, with lower case, underscore-as-separator function names.

The automagic UClass and UEnums mappers
---------------------------------------

Instead of doing a gazilion of unreal_engine.find_class(name) calls, the plugin adds a 'magic' module called unreal_engine.classes. It allows to import unreal classes like python classes:

```py
from unreal_engine.classes import ActorComponent, ForceFeedbackEffect, KismetSystemLibrary

...
components = self.uobject.get_owner().GetComponentsByClass(ActorComponent)

...
self.force_feedback = ue.load_object(ForceFeedbackEffect, '/Game/vibrate')
self.uobject.get_player_controller().ClientPlayForceFeedback(self.force_feedback)

...
name = KismetSystemLibrary.GetObjectName(self.actor)
```

the last example, shows another magic feature: static classes function calls. Obviously in this specific case using self.actor.get_name() would have been the best approach, but this feature allows you to access your blueprint function libraries too.

Another example for adding a widget:

```py
from unreal_engine.classes import WidgetBlueprintLibrary

class PythonFunnyActor:
    def begin_play(self):
        WidgetBlueprintLibrary.Create(self.uobject, ue.find_class('velocity_C'))
```

And another complex example using enums, keyword arguments and output values (output values are appended after the return value):

```py

import unreal_engine as ue
from unreal_engine import FVector, FRotator, FTransform, FHitResult
from unreal_engine.classes import ActorComponent, ForceFeedbackEffect, KismetSystemLibrary, WidgetBlueprintLibrary
from unreal_engine.enums import EInputEvent, ETraceTypeQuery, EDrawDebugTrace

...

is_hitting_something, hit_result = KismetSystemLibrary.LineTraceSingle_NEW(self.actor, self.actor.get_actor_location(), FVector(300, 300, 300), ETraceTypeQuery.TraceTypeQuery1, DrawDebugType=EDrawDebugTrace.ForOneFrame)
if is_hitting_something:
    ue.log(hit_result)
```

The ue_site.py file
-------------------

On Editor/Engine start, the ue_site module is tried for import. You should place initialization code there. If the module cannot be imported, you will get a (harmful) message in the logs.

PyPawn
------

This works like PyActor, but this time you generate a new Pawn class (that you can posses with a controller)


The 'World' concept
-------------------

Every uobject is mapped to a world (UWorld in c++). Generally when you play on a Level your objects all live in the same world, but at the same time there could be multiple worlds (for example while testing in the editor there is a world for the editor and one for the simulation)

While it is pretty rare to reference other worlds, you may need to compare the world of two uobject's (for example you may have a reference in your python module to a uobject of a hidden world and you want to check if you need to use it).

The uobject.get_world() function returns a uobject representing the world (the C++ UWorld class)

The uobject api
---------------

Each uobject represent a UObject class of the Engine. This C++ class is basically the root of all the other classes (Actors, Pawns, components, properties ...). Thanks to Unreal Engine reflection system we do not need to implement a python class for each unreal engine class, but for performance reason we expose the most common methods. The uobject system checks for the type of the mapped C++ UObject and will call the method only if it is safe to call it.

Sometime methods are implemented for automatically getting the right object. As an example get_actor_location() when called over a component will automatically retrieve the related actor and will call C++ AActor::GetActorLocation() method over it.

When this automagic approach is too risky, the method will check for the uobject type and will raise an exception in the case of inconsistencies.

Remember, there is no need to implement every single engine class method, the reflection system is powerful enough to be governed only via properties and function calls (check the uobject call() method)

Most-used methods are implemented directly as uobject methods for performance reasons.

You can get the the list of uobject api methods here: https://github.com/20tab/UnrealEnginePython/blob/master/docs/uobject_API.md

Automatic module reloading (Editor only)
----------------------------------------

When in the editor, you can change the code of your modules mapped to proxies without restarting the project. The editor will reload the module every time a PyActor, PyPawn or PythonComponent is instantiated. This is obviously not the best approach. In the future we would like to implement timestamp monitoring on the file to reload only when needed.

Primitives and Math functions
-----------------------------

The plugin exposes FVector, FRotator, FQuat, FColor, FHitResult and a bunch of the internal handles.

Where meaningful math operations are exposed:


```py
import unreal_engine

class ActorGoingUp:
    def begin_play(self):
        # 1 meter by second
        self.speed = 100
    
    def tick(self, delta_time):
        # get the up vector
        up = self.uobject.get_up_vector()
        # get current position
        position = self.uobject.get_actor_location()
        # build a direction vector based on speed
        up_amount = up * self.speed * delta_time)
        # sum the direction to the position
        position += up_amount
        # set the new position
        self.uobject.set_actor_location(new_position)
```

Referencing objects
-------------------

You can use find_class(), find_struct() and find_object() functions to reference already loaded classes/objects.

If you need to reference assets (still) not loaded in the engine you can use load_struct(), load_class() or load_object():

```py
a_struct_data = ue.load_struct('/Game/Data')
ue.log(a_struct_data.as_dict())
```

or to find a specific asset:

```py
texture_class = ue.find_class('Texture2D')
a_specific_texture = ue.load_object(texture_class, '/Game/Textures/logo2')
```

The as_dict() method
--------------------

This special method can be called on any uobject: it will attempt to serialize it to a python dictionary

Navigation
---------

The only exposed navigation-related method is 'simple_move_to_location'. It expects a Pawn with a movement component (like a Character)

```py
class MoveToTargetComponent:

    def begin_play(self):
        # get a 'target point' reference from a pawn public property
        target = self.uobject.get_owner().get_property('target')
        self.uobject.get_owner().simple_move_to_location(target.get_actor_location())
        
    def tick(self, delta_time):
        pass
```

Another example for diablo-like movements (click to move, add this component to a character)

```py
class Walker:
    def begin_play(self):
        self.uobject.show_mouse_cursor()
    def tick(self, delta_time):
        if not self.uobject.is_input_key_down('LeftMouseButton'):
            return
        hit = self.uobject.get_hit_result_under_cursor(ue.COLLISION_CHANNEL_VISIBILITY)
        if not hit:
            return
        # hit is a unreal_engine.FHitResult object
        self.uobject.simple_move_to_location(hit.impact_point)
```

Physics
-------

The 'set_simulate_physics' method is exposed for enabling physic on PrimitiveComponent.

Remember that you cannot enable physics withotu setting the collision presetes accordingly:

```py

# PyActor with a staticmeshcomponent (a sphere)
# when overlapped it became a physic object
class Ball:

    def begin_play(self):
        self.sphere = self.uobject.get_actor_component_by_type(ue.find_class('StaticMeshComponent'))
        
    def tick(self, delta_time):
        pass
    
    def on_actor_begin_overlap(self, me, other_actor):
        # change collision profile
        self.sphere.call('SetCollisionProfileName BlockAll')
        # enable physics
        self.sphere.set_simulate_physics()
        
    # once the object became a physics one, hit event will start arriving
    def on_actor_hit(self, me, other, *args):
        ue.print_string('HIT with ' + other.get_name())
```

TODO: expose more physics functions, expecially the one for applying forces

Timer
-----

A custom python class is exposed (unreal_engine.FTimerHandler) to support UE4 timers:

```py
# create a timer
timer = self.uobject.set_timer(frequency, callable[, loop, initial])
# clear a timer
timer.clear()
# pause a timer
timer.pause()
# unpause a timer
timer.unpause()
```

Fracturing
----------

Fracturing is one of the best features you get for free in unreal engine.

You can apply damage to destructible objects directly from python (more kind of damages will be added soon)

```py
class DestroyMeComponent:

    def begin_play(self):
        # get a reference to a destructible component in the actor
        self.destructible = self.uobject.get_actor_component_by_type(ue.find_class('DestructibleComponent'))
        
    def tick(self, delta_time):
        pass
        
    def explode(self):
        # damage amount
        amount = 1000
        strength = 20000
        position = self.uobject.get_owner().get_actor_location()
        up = self.uobject.get_owner().get_actor_up()
        self.destructible.destructible_apply_damage(amount, strength, position, up)
    
```

you can now call the 'explode' method via blueprints using the 'Call Python Component Method' node

Another approach (way more easier) would be adding a RadialForceComponent and fire it when you want to destroy something:

```py
# get a reference to the RadialForceComponent
self.radial_force = self.uobject.get_owner().get_actor_component_by_type(ue.find_class('RadialForceComponent'))

# fire it !
self.radial_force.call('FireImpulse')
```



Splines
-------

Splines are another amazing UE4 feature.

The following component shows how to move another actor over a spline path:

```py
class Spline:
    def begin_play(self):
        # get a reference to a spline component
        self.spline = self.uobject.get_owner().get_actor_component_by_type(ue.find_class('SplineComponent'))
        # find the length of the spline
        self.max_distance = self.spline.get_spline_length()
        self.distance = 0.0
        # get a reference to the actor to move (as a blueprint property)
        self.actor_to_move = self.uobject.get_owner().get_property('ObjectToMove')

    def tick(self, delta_time):
        if self.distance >= self.max_distance:
            return
        # find the next point on the spline
        next_point = self.spline.get_world_location_at_distance_along_spline(self.distance)
        self.actor_to_move.set_actor_location(next_point)
        self.distance += 100 * delta_time
```

Blueprints integration
----------------------

You can call blueprints functions (or custom events) via the .call() and .call_function() methods:

```py
your_funny_blueprint_object.call('AFunctionOrACustomEvent with_a_arg')
```

Whenever you need to reference external object, avoid using find_object() and similar. Instead add a public variable in your blueprint
pointing to the specific object. You can then reference this object easily getting the property value:

```py
the_other_object = self.uobject.get_property('target')
the_other_object.set_actor_location(0, 0, 0)
```

.call_function() is more advanced, as it allows for return values and python args:

```py
# an example of moving an object z with curves:
class Curver:
    def begin_play(self):
        self.curve = self.uobject.get_owner().get_property('curve')
        self.accumulator = 0.0
    def tick(self, delta_time):
        location = self.uobject.get_actor_location()
        z = self.curve.call_function('GetFloatValue', self.accumulator) * 100
        self.uobject.set_actor_location(location.x, location.y, z)
        self.accumulator += delta_time

```


Events
------

You can easily bind events (as seen before) with the bind_event function

```py
self.uobject.bind_event('OnActorBeginOverlap', a_funny_callback)
```

Support for mapping custom events directly from python is currently worked on.

If you want to map events from a blueprint to a python function, the best thing to do is using the 'python call' blueprint functions exposed by the various plugin classes:

![Alt text](screenshots/unreal_screenshot3.png?raw=true "Screenshot 3")

Audio
-----

The uobject.play_sound_at_location(sound, position[, volume_multiplier, pitch_multiplier, start_time]) api method is exposed:

```py
# get a reference to asound
sound = ue.find_object('my_sound')
# play the sound at position 0,0,0
self.uobject.play_sound_at_location(sound, FVector(0, 0, 0))
```

If you prefer to work with AudioComponent:

```py
class Sounder:
    def begin_play(self):
        # find the AudioComponent of this actor
        self.audio = self.uobject.get_component_by_type('AudioComponent')
        self.audio.call('Stop')
    def tick(self, delta_time):
        # start the sound when pressing 'A'
        if self.uobject.is_input_key_down('A'):
            self.audio.call('Play')
```

Animations
----------

You can control animation blueprints variables and events easily:

```py
# get a reference to the skeletal mesh
skeletal = self.uobject.get_component_by_type('SkeletalMeshComponent')
# get a reference to the animation class
animation = skeletal.get_anim_instance()

# set a variable
animation.set_property('Speed', 17.0)

# trigger a custom event
animation.call('AttackWithSword')
```

Packaging
---------

When you package your projects, remember to include the libpython (dll or dylib) in the binaries folder and the Scripts directory.

If you do not want to distribute python sources, you can include only the ```__pycache__``` directory with the bytecode.

Do not forget to include python third party modules (if you use any of them in your project)

Examples
--------

This is a PyActor destroying itself whenever another actor overlap it. Remember to add a mesh component to it (like a sphere) and set its collision behaviour as 'OverlapAll'. This could be tested with the third person official template.

```py
class Ball:
    def begin_play(self):
        ue.print_string('Hello')
    def tick(self, delta_time):
        pass
    def on_actor_begin_overlap(self, other_actor):
        ue.print_string('Collided with ' + other_actor.get_name())
        self.uobject.actor_destroy()
```

Now we create (at runtime !!!) a whole new PyActor:

```py
class SuperHero:
    def begin_play(self):
        # spawn a new PyActor
        new_actor = self.uobject.actor_spawn(ue.find_class('PyActor'), Fvector(0, 0, 0),FRotator(0, 0, 90))
        # add a sphere component as the root one
        static_mesh = new_actor.add_actor_root_component(ue.find_class('StaticMeshComponent'), 'SphereMesh')
        # set the mesh as the Sphere asset
        static_mesh.call('SetStaticMesh /Engine/EngineMeshes/Sphere.Sphere')
        # set the python module
        new_actor.set_property('PythonModule', 'gameclasses')
        # set the python class
        new_actor.set_property('PythonClass', 'Vertical')
        
    def tick(self, delta_time):
        pass
```

Integration with PyQT
---------------------

To correctly integrates PyQT with UnrealEngine the python plugin must correctly setup the GIL (and this is done) and exceptions must be managed ad-hoc (not doing it will result in a deadlock whenever a qt signal handler raises an exception)

This is an example of having a QT window along the editor to trigger asset reimporting (pay attention to the sys.excepthook usage):

```py
from PyQt5.QtWidgets import QApplication, QWidget, QListWidget
import unreal_engine as ue
import sys
import traceback

def ue_exception(_type, value, back):
    ue.log_error(value)
    tb_lines = traceback.format_exception(_type, value, back)
    for line in tb_lines:
        ue.log_error(line)

sys.excepthook = ue_exception

skeletal_mappings = {}

def selected_skeletal_mesh(item):
    uobject = skeletal_mappings[item.data()]
    ue.log('Ready to reimport: ' + uobject.get_name())
    uobject.asset_reimport()

app = QApplication([])

win = QWidget()
win.setWindowTitle('Unreal Engine 4 skeletal meshes reimporter')

wlist = QListWidget(win)
for asset in ue.get_assets_by_class('SkeletalMesh'):
    wlist.addItem(asset.get_name())
    skeletal_mappings[asset.get_name()] = asset
    
wlist.clicked.connect(selected_skeletal_mesh)
wlist.show()

win.show()
```

Memory management
-----------------

Dealing with 2 different GC's is really challenging.

PyActor, PyPawn and PythonComponent automatically DECREF's the mapped classes. This should be enough unless you hold references
in the python modules themselves. As this would be a bad programming practice, the current approach should be safe enough.

In addition to this, every time a uobject has to return its UObject mapping, it checks for its validity and safety:

```c
#define ue_py_check(py_u) if (!py_u->ue_object || !py_u->ue_object->IsValidLowLevel() || py_u->ue_object->IsPendingKillOrUnreachable())\
							return PyErr_Format(PyExc_Exception, "PyUObject is in invalid state")
```


Unit Testing
------------

The repository includes a TestingActor asset and a tests.py script. Just add the asset and the script to your project and the test suite will be run. The test suite is still a prototype it will be improved soon.

Threading (Experimental)
------------------------

By default the plugin is compiled without effective python threads support. This is for 2 main reasons:

* we still do not have numbers about the performance impact of constantly acquiring and releasing the GIL
* we need a better test suite

By the way, if you want to play with experimental threading support, just uncomment

```c
//#define UEPY_THREADING 1
```

on top of UnrealEnginePythonPrivatePCH.h and rebuild the plugin.

As with native threads, do not modify (included deletion) UObjects from non-main threads.

Accessing Python Proxy From UObject
-----------------------------------

Sometimes you may have a UObject and know that it is backed by a python object. To get the python object from the UObject, use the `get_py_proxy` method. For example, imagine you have the following situation:

   1. There is a `PyActor` sub-class called `PyExplosiveActor` which has `Explosive` as its python class.
   2. The `Explosive` has a `go_boom` python method.
   3. There is a `PyActor` sub-class called `PyBadGuyActor` which has a Blueprint property called `MyBomb` and a python class called `BadGuy`.
   4. The `BadGuy` instance in python knows that its UObject has its `MyBomb` as an instance of `PyExplosiveActor` and wants to call the `go_boom` python method.
   
This would be resolved as shown below:

```
import unreal_engine as ue

class Explosive:
    'Python representation for PyExplosiveActor in UE4'

    def go_boom(self):
        # do python stuff to explode
        ...
        self.uobject.destory()

class BadGuy:
    'Python reprsentation for PyBadGuyActor in UE4'
   
    def ignite_bomb(self, delay):
        bomb = self.uobject.MyBomb
        py_bomb = bomb.get_py_proxy()
        py_bomb.go_boom()
	
```

What is going on here in `BadGuy` is that self.uobject is a reference to the PyActor UObject and `self.uobject.MyBomb` is a reference to the `PyExplosive` uobject. But instead you want to access its proxy class (`Explosive`). The `get_py_proxy()` method returns the python custom class, `Explosive` that the `PyExplosiveActor` object is mapped to.

Status and Known issues
-----------------------

The project could be considered in beta state.

Exposing the full ue4 api is a huge amount of work, feel free to make pull requests for your specific needs.

.py files are not recognized by the editor. This should be fixed soon

We still do not have a plugin icon ;)

The build system is not very robust. Maybe linking the python static library into the plugin dll could be a better approach.


Contacts and Commercial Support
-------------------------------

If you want to contact us (for help, support, sponsorship), drop a mail to info at 20tab.com or follow @unbit on twitter

We offer commercial support for both UnrealEngine and UnrealEnginePython, again drop a mail to info at 20tab.com for more infos
