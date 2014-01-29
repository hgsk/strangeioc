## Strange: Unity3DとC#のためのUnityのためのIoCフレームワーク

現在のバージョン: v0.7.0

> Strange アトラクタはおおくの混沌としたシステムで予測可能なパターンを作成します。

StrangeはC＃とユニティのために特別に書かれた、超軽量で拡張性の高い反転·オブ·コントロール（IOC）のフレームワークです。 我々は、Web、スタンドアロン、およびiOSとAndroid上でStrangeを検証してきました。 Android用の開発はしていませんが、問題なく実行できると思います。(動かなかったら教えてください！)

* [概要](http://thirdmotion.github.com/strangeioc/exec.html)
* [ドキュメント](http://thirdmotion.github.com/strangeioc/docs/html/index.html)
* [How To](http://thirdmotion.github.com/strangeioc/TheBigStrangeHowTo.html)
* [ロボットレッグを使っていますか？それならStrangeはあなたのためにあります！](http://thirdmotion.github.com/strangeioc/rl.html)
* [FAQ](http://thirdmotion.github.com/strangeioc/faq.html)
* [機能リクエスト/バグレポート](https://github.com/thirdmotion/strangeioc/issues)

Strangeは以下の機能を含みます。（ほとんどはオプションです）

* バインディングフレームワーク
* DI
  * シングルトン、value、ファクトリのマッピング（新しいインスタンスごとに取得）
  * 名前のインジェクション
  * コンストラクタインジェクション、setterインジェクションの実行
  * お好みのコンストラクタにタグ付け
  * オブジェクト生成後に実行するメソッドのタグ付け
  * MonoBehavioursへのインジェクション
  * 多態バインディング (あらゆるインターフェースの、具象クラスへのバインディング)
* リフレクション機構のオーバーヘッドを劇的に軽減する、リフレクションバインディング
* ２通りのイベントバスシステム: EventDispatcher, Signal
  これにより、
  * あらゆる場所にイベントをマッピング
  * ローカル通信のためのローカルイベントをマッピング
  * イベントをCommandクラスにマッピングし、ビジネスロジックを分離
  * EventDispatcherはペイロードデータをプリミティブ、またはVOとして転送できます
  * シグナルはデータをバインダブルで、型安全なパラメータとして転送します
* MonoBehaviourの仲介
  * アプリケーションからビューの分離を容易に
  * Unity固有のコードを分離
* MVCS (Model/View/Controller/Service) （オプショナル）
* 複数のコンテキスト
  * サブコンポーネントを (シーン分離) その中だけで、またはさらに大きなアプリケーションの中で機能するようにできます
  * コンテクスト間の通信
* 必要なものがない？ コアバインディングフレームワークを拡張するのは簡単です。 次のようなバインダを構築できます:
  * AS3-Signalsのような違うタイプのディスパッチャ （もうちょい待って！作りましたので！）
  * エンティティフレームワーク
  * マルチローダー

Strangeには、あなたのプロジェクトを賢い構造にするだけでなく、次のような利点もあります。

* Unity3Dで扱いやすいように設計（Unity3D無しでもうまく動きます。）
* UnityEngineのコードをアプリケーションから分離
  * ポータビリティの向上
  * ユニットテスト容易性の向上
* 共通イベントバスが情報の流れを簡単かつ高度に分離（注: UnityのSendMessageメソッドもありますが、わりと危険です。この点については全体を通して触れていきます）
* 拡張可能なバインダは本当に便利です。 (友人は「自分のキッチンみたいに便利」と言ってくれました） 
この小さなコアフレームワークで実現できることは多く、Strangeの独自性を認めてもらえると思います。
* 複数コンテクストはサブコンポーネントを「bootstrap」できます。
サブコンポーネントは単独でも動作しますし、部品としても動きます。
これによりあなた自身の開発や、複数人による分散開発と開発終盤での統合をスピードアップできます。
* コードからプラットフォーム固有の #IF...#ELSE 構造を取り除くことができます. IoCはすべての具象化の実装を正しく書けるするようにでき、使いたいものをコンパイル時、ランタイム時にバインドできます。 (バインディングの別の形として #IF...#ELSE 構文 をアプリケーションから単一クラスに分離できます。)

# 謝辞
It is hard to adequately credit the creators of the open source Actionscript framework RobotLegs for their influence on the creation of StrangeIoC. While Strange is not a port of RobotLegs, the ensigns of that library are copiously reflected throughout this one. For their great service to my professional development, I offer that team my sincerest thanks. And a donut. Seriously, if you're ever in town, let me buy you a donut.

Kudos to Will Corwin for picking up a thrown-down gauntlet and writing the Signals implementation.

I also need to thank and congratulate the folks at [ThirdMotion](http://www.thirdmotion.com) who inexplicably gave me time to build Strange and license to open source it.

/**********************************************/
  Copyright 2013 ThirdMotion, Inc.
 
 	Licensed under the Apache License, Version 2.0 (the "License");
 	you may not use this file except in compliance with the License.
 	You may obtain a copy of the License at
 
 		http://www.apache.org/licenses/LICENSE-2.0
 
 		Unless required by applicable law or agreed to in writing, software
 		distributed under the License is distributed on an "AS IS" BASIS,
 		WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 		See the License for the specific language governing permissions and
 		limitations under the License.
 
