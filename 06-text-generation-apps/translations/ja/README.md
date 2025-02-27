# テキスト生成アプリケーションの構築

[![テキスト生成アプリケーションの構築](../../images/06-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/5jKHzY6-4s8?WT.mc_id=academic-105485-koreyst)

> このレッスンのビデオを見るには上の画像をクリックしてください。

このカリキュラムを通して、プロンプトのような核となる概念や、「プロンプト・エンジニアリング」と呼ばれる学問分野全体があることを、これまで見てきました。ChatGPT、Office 365、Microsoft Power Platformなど、多くのツールはプロンプトを使って何かを達成することをサポートしています。

このようなエクスペリエンスをアプリに追加するには、プロンプトや補完などの概念を理解し、使用するライブラリを選択する必要があります。この章では、まさにこのことを学びます。

## はじめに

この章では

- openaiライブラリとそのコアコンセプトについて学ぶ。
- openaiを使ってテキスト生成アプリを作る。
- プロンプト、温度、トークンなどの概念をどのように使ってテキスト生成アプリを作るかを理解する。

## 学習目標

このレッスンの終わりには、以下のことができるようになります：

- テキスト生成アプリとは何かを説明できる
- openaiを使用してテキスト生成アプリを構築する。
- openaiを使ってテキスト生成アプリを作ることができる。

## テキスト生成アプリとは何か？

通常、アプリをビルドするときは、以下のようなインターフェイスを持つ：

- コマンドベース。コンソールアプリは、コマンドを入力するとタスクが実行される典型的なアプリです。例えば、`git`はコマンドベースのアプリです。
- ユーザーインターフェース（UI）。ボタンをクリックしたり、テキストを入力したり、オプションを選択したりするグラフィカルユーザーインターフェース（GUI）を持つアプリもあります。

### コンソールアプリやUIアプリには制限があります。

コマンドを入力するコマンドベースのアプリと比べてみてください：

- 限定的**です。どんなコマンドでも入力できるわけではなく、アプリがサポートしているものだけです。
- 言語が限定されている**。アプリによっては多くの言語に対応していますが、デフォルトでは特定の言語向けに作られています。

### テキスト生成アプリの利点

では、テキスト生成アプリはどう違うのでしょうか？

テキスト生成アプリでは、より柔軟性があり、一連のコマンドや特定の入力言語に制限されません。その代わりに、自然言語を使用してアプリと対話することができます。もう1つの利点は、従来のアプリがデータベースにあるものに限定されるかもしれないのに対し、膨大な情報のコーパスで訓練されたデータソースとすでにやりとりしていることです。

### テキスト生成アプリで何が作れるか？

作れるものはたくさんある。例えば

- チャットボット**。あなたの会社や製品のようなトピックに関する質問に答えるチャットボットは、相性が良いかもしれません。
- ヘルパー**。LLMは、テキストを要約したり、テキストから洞察を得たり、履歴書などのテキストを作成したりするようなことが得意です。
- コード・アシスタント**。使用する言語モデルに応じて、コードを書くのを助けるコード・アシスタントを構築することができる。例えば、ChatGPTと同様にGitHub Copilotのような製品を使用して、コードを書くのを助けることができます。

## どうすれば始められますか？

LLMと統合する方法を見つける必要がありますが、それには通常次の2つのアプローチがあります：

- APIを使う。APIを使う。プロンプトでウェブリクエストを作成し、生成されたテキストを返す。
- ライブラリを使う。ライブラリはAPIコールをカプセル化し、使いやすくする。

## ライブラリ/SDK

LLMを扱うための有名なライブラリがいくつかある：

- openai**、このライブラリを使えば、モデルへの接続やプロンプトの送信を簡単に行うことができます。

それから、より高いレベルで動作するライブラリもあります：

- **Langchain**です。Langchainはよく知られており、Pythonをサポートしています。
- **Semantic Kernel**. Semantic KernelはMicrosoftによるライブラリで、C#、Python、Javaをサポートしている。

## openaiを使った最初のアプリ

どのように最初のアプリを作るのか、どんなライブラリーが必要なのか、どのくらい必要なのか、などを見てみよう。

### openaiのインストール

OpenAIやAzure OpenAIとやりとりするためのライブラリはたくさんあります。C#、Python、JavaScript、Javaなど、多くのプログラミング言語を使うことができます。 ここでは `openai` Python ライブラリを使うことにしたので、`pip` を使ってインストールする。

```bash
pip install openai
```

### リソースの作成

以下のステップを実行する必要があります：

- Azure [https://azure.microsoft.com/free/](https://azure.microsoft.com/free/?WT.mc_id=academic-105485-koreyst) でアカウントを作成する。
- Azure Open AIにアクセスする。https://learn.microsoft.com/azure/ai-services/openai/overview#how-do-i-get-access-to-azure-openai](https://learn.microsoft.com/azure/ai-services/openai/overview#how-do-i-get-access-to-azure-openai?WT.mc_id=academic-105485-koreyst) にアクセスし、アクセスをリクエストする。

  > 注意
  > 執筆時点では、Azure Open AIへのアクセスを申請する必要があります。

- Python のインストール <https://www.python.org/>
- Azure OpenAI Service リソースを作成しました。リソースの作成](https://learn.microsoft.com/azure/ai-services/openai/how-to/create-resource?pivots=web-portal?WT.mc_id=academic-105485-koreyst) の方法は、こちらのガイドを参照してください。

### API キーとエンドポイントの確認

この時点で、使用する API キーを `openai` ライブラリに伝える必要があります。API キーを見つけるには、Azure Open AI リソースの "Keys and Endpoint" セクションに行き、"Key 1" の値をコピーします。

![Azureポータルのキーとエンドポイント・リソース・ブレード](https://learn.microsoft.com/azure/ai-services/openai/media/quickstarts/endpoint.png)

この情報をコピーできたので、それを使うようにライブラリに指示しよう。

> 注意
> APIキーをコードから分離する価値はある。環境変数を使うことで、そうすることができます。
>
> 環境変数 `OPENAI_API_KEY` にAPIキーを設定する。
> OPENAI_API_KEY='sk-...'` をエクスポートする。

### セットアップ構成 Azure

Azure Open AI を使っている場合の設定方法です：

```python
openai.api_type = 'azure'
openai.api_key = os.environ["OPENAI_API_KEY"]
openai.api_version = '2023-05-15'
openai.api_base = os.getenv("API_BASE")
```

上記では以下のように設定している：

- `api_type`を`azure`に設定している。これはライブラリに OpenAI ではなく Azure Open AI を使うように指示する。
- `api_key`: Azure Portal にある API キー。
- `api_version`, 使用する API のバージョンである。この記事を書いている時点の最新バージョンは `2023-05-15` である。
- `api_base`, API のエンドポイントである。AzureポータルのAPIキーの横に記載されている。

> 注意
> `o.getenv` は環境変数を読み込む関数です。これを使用して、`OPENAI_API_KEY` や `API_BASE` などの環境変数を読み込むことができます。これらの環境変数はターミナルで設定するか、 `dotenv` のようなライブラリを使用して設定する。

## テキストの生成

テキストを生成するには `Completion` クラスを使用する。以下に例を示す：

```python
prompt = "以下を完成させなさい： 昔々、あるところに"

completion = openai.Completion.create(model="davinci-002", prompt=prompt)
print(completion.choices[0].text)
```

上記のコードでは、補完オブジェクトを作成し、使用したいモデルとプロンプトを渡している。そして、生成されたテキストを表示します。

### チャットの補完

ここまでは `Completion` を使ってテキストを生成する方法を見てきました。しかし、チャットボットに適した `ChatCompletion` というクラスもあります。これを使う例を示します：

```python
import openai

openai.api_key = "sk-..."

completion = openai.ChatCompletion.create(model="gpt-3.5-turbo", messages=[{"role": "user", "content": "Hello world"}])
print(completion.choices[0].message.content)
```

この機能については、次の章で詳しく説明します。

## 練習 - 初めてのテキスト生成アプリ

openaiのセットアップと設定方法を学んだので、いよいよ最初のテキスト生成アプリを作ってみましょう。アプリを作るには、以下の手順に従います：

1. 仮想環境を作成し、openaiをインストールします：

    ```bash
    python -m venv venv
    source venv/bin/activate
    pip install openai
    ```

    > [!注意]。
    > Windowsを使用している場合、`source venv/bin/activate`の代わりに`venvScripts\activate`と入力してください。

    > [!注意]
    > [https://portal.azure.com/](https://portal.azure.com/?WT.mc_id=academic-105485-koreyst) にアクセスし、`Open AI` を検索して Azure Open AI キーを見つけ、`Open AI resource` を選択し、`Keys and Endpoint` を選択して `Key 1` の値をコピーします。

1.  *app.py* ファイルを作成し、以下のコードを記述します：


    ```python
    import openai

    openai.api_key = "<replace this value with your open ai key or Azure Open AI key>"

    openai.api_type = 'azure' 
    openai.api_version = '2023-05-15'
    openai.api_base = "<endpoint found in Azure Portal where your API key is>"
    deployment_name = "<deployment name>"

    # add your completion code
    prompt = "以下を完成させなさい： 昔々、あるところに"

    # make completion
    completion = openai.Completion.create(engine= deployment_name, model="davinci-002", prompt=prompt)
    
    # print response
    print(completion.choices[0].text)
    ```

    > 注意
    > Azure Open AI を使用している場合は、`api_type` を `azure` に設定し、`api_key` に Azure Open AI のキーを設定する必要があります。

    以下のような出力が表示されるはずです：

    ```output
     とても不幸な＿＿＿。

    昔々、とても不幸な人魚がいた。
    ```

## 異なるタイプのプロンプト、異なるものに対して

これで、プロンプトを使ってテキストを生成する方法を見たことになる。プロンプトはあらゆる種類のタスクに使うことができる。

プロンプトはあらゆる種類のタスクに使うことができる。例えば

- テキストを生成してください。例えば、詩やクイズの問題などを生成することができます。
- **情報の検索**。プロンプトを使用して、次の例のように情報を検索することができます 'Web開発でCORSは何を意味するのでしょうか？
- **コードの生成**。プロンプトを使用してコードを生成することができます。たとえば、電子メールの検証に使用する正規表現を開発したり、Web アプリのようなプログラム全体を生成したりすることができます。

## より実用的な使用例: レシピジェネレーター

家に材料があり、何かを作りたいとします。そのためにはレシピが必要です。レシピを探すには、検索エンジンを使うか、LLMを使うのがよいでしょう。

このようにプロンプトを書くことができる：

> 「鶏肉、ジャガイモ、ニンジン。レシピごとに、使用されている材料をすべて挙げてください。」

上記のような回答が返ってくるかもしれません：

```output
1. チキンと野菜のロースト 
材料 
- 鶏もも肉4枚
- ジャガイモ2個（角切り
- にんじん 2本（角切り
- オリーブオイル 大さじ2
- ニンニク（みじん切り） 2片
- 乾燥タイム 小さじ1
- 乾燥オレガノ 小さじ1
- 塩、こしょう 各適量

2. チキンとジャガイモのシチュー 
材料 
- オリーブオイル 大さじ2
- 玉ねぎ（さいの目切り）1個
- ニンニク 2片（みじん切り
- 鶏胸肉 2枚（角切り
- ジャガイモ 2個（角切り
- にんじん 2本（角切り
- 乾燥オレガノ 小さじ1
- 乾燥タイム 小さじ1
- 鶏ガラスープ 1カップ
- 塩・胡椒 適量

3. チキンとポテトのベイク 
材料 
- オリーブオイル 大さじ2
- 鶏胸肉（角切り） 2枚
- ジャガイモ 2個（角切り
- にんじん 2本（角切り
- 玉ねぎ（さいの目切り） 1個
- にんにく（みじん切り） 2片
- 乾燥オレガノ 小さじ1
- 乾燥タイム 小さじ1
- チキンスープ 1カップ
- 塩、こしょう 適量

4. チキンとポテトのスープ 
材料 
- オリーブオイル 大さじ2
- 玉ねぎ（さいの目切り）1個
- ニンニク 2片（みじん切り
- 鶏胸肉 2枚（角切り
- ジャガイモ 2個（角切り
- にんじん 2本（角切り
- 乾燥オレガノ 小さじ1
- 乾燥タイム 小さじ1
- 鶏ガラスープ 4カップ
- 塩・胡椒 適量

5. チキンとポテトのハッシュ 
材料 
- オリーブオイル 大さじ2
- 鶏胸肉 2枚（角切り
- ジャガイモ 2個（角切り
- にんじん 2本（角切り
- 玉ねぎ（さいの目切り） 1個
- にんにく（みじん切り） 2片
- 乾燥オレガノ 小さじ1
```

この結果は素晴らしい。この時点で、役に立つ改善点は以下の通りだ：

- 嫌いな食材やアレルギーのある食材を除外する。
- 家にすべての材料がない場合に備えて、買い物リストを作成する。

上記のケースについて、追加のプロンプトを追加しよう：

> ニンニクアレルギーのため、ニンニクを使ったレシピを削除し、他のものに置き換えてください。また、家に鶏肉、ジャガイモ、ニンジンがあることを考慮して、レシピの買い物リストを作成してください。"

これで新しい結果が出た：

```output
1. チキンと野菜のロースト 
材料 
- 鶏もも肉4枚
- ジャガイモ2個（角切り
- にんじん 2本（角切り
- オリーブオイル 大さじ2
- 乾燥タイム 小さじ1
- 乾燥オレガノ 小さじ1
- 塩・胡椒 適量

2. チキンとジャガイモのシチュー 
材料 
- オリーブオイル 大さじ2
- 玉ねぎ（さいの目切り） 1個
- 鶏胸肉 2枚（角切り
- ジャガイモ 2個（角切り
- にんじん 2本（角切り
- 乾燥オレガノ 小さじ1
- 乾燥タイム 小さじ1
- 鶏ガラスープ 1カップ
- 塩・胡椒 適量

3. チキンとポテトのベイク 
材料 
- オリーブオイル 大さじ2
- 鶏胸肉（角切り） 2枚
- ジャガイモ 2個（角切り
- にんじん 2本（角切り
- 玉ねぎ（さいの目切り） 1個
- 乾燥オレガノ 小さじ1
- 乾燥タイム 小さじ1
- チキンスープ 1カップ
- 塩・胡椒 適量

4. チキンとポテトのスープ 
材料 
- オリーブオイル 大さじ2
- 玉ねぎ（さいの目切り） 1個
- 鶏胸肉 2枚（角切り
- ジャガイモ 2個（角切り
- にんじん 2本（角切り
- 乾燥オレガノ 小さじ1
- 乾燥タイム 小さじ1
- 鶏ガラスープ 4カップ
- 塩・胡椒 適量

5. チキンとポテトのハッシュ 
材料 
- オリーブオイル 大さじ2
- 鶏胸肉 2枚（角切り
- ジャガイモ 2個（角切り
- にんじん 2本（角切り
- 玉ねぎ（さいの目切り） 1個
- 乾燥オレガノ 小さじ1

買い物リスト 
- オリーブオイル
- 玉ねぎ
- タイム
- オレガノ
- 塩
- 胡椒
```

これが5つのレシピで、ニンニクについては触れていない。

## 練習 - レシピジェネレーターを作る

さて、シナリオを演じてみたので、デモンストレーションされたシナリオにマッチするコードを書いてみよう。そのためには、以下のステップに従ってください：

1. 既存の *app.py* ファイルを出発点として使います。
1. 変数 `prompt` を探し、そのコードを以下のように変更します：

    ```python
    prompt = "鶏肉、ジャガイモ、ニンジンを使った料理のレシピを5つ教えてください。レシピ1つにつき、使用されている材料をすべて挙げてください。"
    ```

    このコードを実行すると、次のような出力が表示されるはずだ：

    ```output
    -ジャガイモとニンジンのチキンシチュー： 油大さじ3、玉ねぎ（みじん切り）1個、にんにく（みじん切り）2片、にんじん（皮をむいてみじん切り）1本、じゃがいも（皮をむいてみじん切り）1個、ローリエ1枚、タイム1枝、塩小さじ1/2、黒こしょう小さじ1/4、鶏ガラスープ1/2カップ、辛口白ワイン1/2カップ、みじん切り生パセリ大さじ2、無塩バター大さじ2、骨なし皮なし鶏もも肉1/2ポンド（1インチ幅に切る
    -ポテトとニンジン入り鶏肉のオーブン焼き：エクストラバージンオリーブオイル大さじ3、ディジョンマスタード大さじ1、ローズマリー（みじん切り）大さじ1、タイム（みじん切り）大さじ1、ニンニク（みじん切り）4片、赤ジャガイモ（小4等分）1.5ポンド、ニンジン（縦4等分）1.5ポンド、塩小さじ1/2、黒コショウ小さじ1/4、丸鶏1羽（4ポンド
    -鶏肉、じゃがいも、にんじんのキャセロール：クッキングスプレー、玉ねぎ（みじん切り）大1個、にんにく（みじん切り）2片、にんじん（皮をむいて千切り）1本、じゃがいも（皮をむいて千切り）1個、乾燥タイムの葉小さじ1/2、塩小さじ1/4、黒こしょう小さじ1/4、無脂肪減塩チキンスープ2カップ、冷凍えんどう豆1カップ、万能小麦粉1/4カップ、2％減脂肪牛乳1カップ、すりおろしたパルメザンチーズ1/4カップ
    
    -ワンポットチキンとポテトのディナー：オリーブオイル大さじ2、骨なし皮なし鶏もも肉1ポンド（1インチ幅に切る）、玉ねぎ大1個（みじん切り）、にんにく3片（みじん切り）、にんじん1本（皮をむいてみじん切り）、じゃがいも1個（皮をむいてみじん切り）、ローリエ1枚、タイム1枝、塩小さじ1/2、黒こしょう小さじ1/4、チキンスープ2カップ、辛口白ワイン1/2カップ
    
    -チキンとジャガイモとニンジンのカレー： 植物油大さじ1、玉ねぎ（みじん切り）大1個、にんにく（みじん切り）2片、にんじん（皮をむいてみじん切り）1本、じゃがいも（皮をむいてみじん切り）1個、コリアンダー（すりおろし）小さじ1、クミン（すりおろし）小さじ1、ターメリック（すりおろし）小さじ1/2、 生姜（すりおろし）小さじ1/2、カイエンヌペッパー小さじ1/4、チキンスープ2カップ、白ワイン（辛口）1/2カップ、水切りして洗ったひよこ豆缶（15オンス）1缶、レーズン1/2カップ、生のコリアンダーのみじん切り1/2カップ
    ```

    > 注：LLMは非決定論的なので、プログラムを実行するたびに異なる結果が出るかもしれません。

    では、どのように改善するか考えてみよう。レシピの材料や数を改良したり変更したりできるように、柔軟なコードにしたい。

1. コードを次のように変更してみよう：

    ```python
    no_recipes = input("レシピ数（例：5")

    ingredients = input("材料のリスト（例えば、鶏肉、ジャガイモ、ニンジン： ")
    
    # レシピの数を食材のプロンプトに補間する
    prompt = f"次の材料を使った料理のレシピを{no_recipes}教えてください： {ingredients}。レシピごとに、使用する材料をすべて挙げてください。"
    ```

    テスト実行のコードを例にとると、次のようになる。:

    ```output
    レシピ数（例：5：3
    材料リスト（例：鶏肉、じゃがいも、にんじん：牛乳、いちご

    -イチゴミルクシェイク：牛乳、イチゴ、砂糖、バニラエッセンス、角氷
    -イチゴのショートケーキ：牛乳、小麦粉、ベーキングパウダー、砂糖、塩、無塩バター、イチゴ、生クリーム。       
    -ストロベリーミルク：牛乳、イチゴ、砂糖、バニラエッセンス
    ```

### フィルターと買い物リストを追加して改善

現在、レシピを作成できるアプリが動作しており、レシピの数だけでなく、使用する食材もユーザーからの入力に依存しているため、柔軟性がある。

さらに改善するために、次のことを追加したいと思います：

- **食材のフィルタリング**。嫌いな食材やアレルギーのある食材をフィルタリングできるようにしたい。この変更を実現するには、既存のプロンプトを編集し、その最後にフィルター条件を追加します：

    ```python
    filter = input("フィルター（ベジタリアン、ビーガン、グルテンフリーなど： ")

    prompt = f"次の材料を使った料理のレシピを{no_recipes}教えてください： {ingredients}。レシピごとに、使用するすべての材料をリストアップしてください。"
    ```

    上記では、プロンプトの最後に `{filter}` を追加し、ユーザーからのフィルター値も取り込んでいる。

    プログラムを実行する入力の例は次のようになる：

    ```output
    レシピ数（例：5：3
    材料リスト（例：鶏肉、じゃがいも、にんじん：玉ねぎ、牛乳
    フィルター（例：ベジタリアン、ビーガン、グルテンフリー：牛乳なし

    1. フレンチ・オニオン・スープ

    材料
    
    -スライスした玉ねぎ大1個
    -ビーフ・ブロス3カップ
    -牛乳1カップ
    -フランスパン6枚
    -パルメザンチーズ 1/4カップ
    -バター 大さじ1
    -乾燥タイム 小さじ1
    -塩 小さじ1/4
    -黒こしょう 小さじ1/4
    
    作り方
    
    1. 大きな鍋にバターを入れ、玉ねぎをきつね色になるまで炒める。
    2. ビーフスープ、牛乳、タイム、塩、胡椒を加える。沸騰させる。
    3. 火を弱め、10分間煮る。
    4. スライスしたフランスパンをスープボウルにのせる。
    5. スープをパンにかける。
    6. パルメザンチーズを振りかける。
    
    2. タマネギとジャガイモのスープ
    
    材料
    
    -玉ねぎ（みじん切り） 大1個
    -ジャガイモ（さいの目切り）2カップ
    -野菜スープ 3カップ
    -牛乳1カップ
    -黒こしょう 小さじ1/4
    
    作り方
    
    1. 大きな鍋にバターを入れ、玉ねぎをきつね色になるまで炒める。
    2. じゃがいも、野菜スープ、牛乳、こしょうを加える。沸騰させる。
    3. 火を弱め、10分間煮る。
    4. 熱いうちにサービスする。
    
    3. クリーミーオニオンスープ
    
    材料
    
    -玉ねぎ大1個（みじん切り
    -野菜スープ3カップ
    -牛乳1カップ
    -黒こしょう 小さじ1/4
    -強力粉 1/4カップ
    -パルメザンチーズ 1/2カップ
    
    作り方
    
    1. 大きな鍋にバターを入れ、玉ねぎをきつね色になるまで炒める。
    2. 野菜スープ、牛乳、コショウを加える。沸騰させる。
    3. 火を弱め、10分間煮る。
    4. 小さなボウルに小麦粉とパルメザンチーズを入れ、なめらかになるまで泡立てる。
    5. スープに加え、さらに5分、またはスープにとろみがつくまで煮る。
    ```

    ご覧の通り、牛乳を使ったレシピはすべてフィルタリングされています。しかし、もしあなたが乳糖不耐症なら、チーズの入ったレシピもフィルターにかけたほうがいいかもしれないので、はっきりさせる必要がある。


- 買い物リストを作る すでに家にあるものを考慮して、買い物リストを作りたい。

    この機能については、1つのプロンプトですべてを解決しようとするか、2つのプロンプトに分けるかのどちらかだ。後者の方法を試してみよう。ここでは、追加のプロンプトを追加することを提案しているが、そのためには、前者のプロンプトの結果を後者のプロンプトのコンテキストとして追加する必要がある。

    最初のプロンプトの結果を出力する部分をコードから探し出し、以下のコードを追加します：

    ```python
    old_prompt_result = completion.choices[0].text
    prompt = "作成したレシピの買い物リストを作成し、私がすでに持っている材料を含めないようにしてください。"
    
    new_prompt = f"{old_prompt_result} {prompt}"
    completion = openai.Completion.create(engine=deployment_name, prompt=new_prompt, max_tokens=1200)
    
    # print response
    print("Shopping list:")
    print(completion.choices[0].text)
    ```

    次のことに注意してほしい：

    1. 最初のプロンプトの結果を新しいプロンプトに追加することで、新しいプロンプトを作成している：

        ```python
        new_prompt = f"{old_prompt_result} {prompt}"
        ```

    1. 新しいリクエストを行うが、最初のプロンプトで要求したトークン数も考慮する。今回は `max_tokens` を1200とする。

        ```python
        completion = openai.Completion.create(engine=deployment_name, prompt=new_prompt, max_tokens=1200)
        ```  

        このコードをちょっと試してみると、次のような出力が得られる：

        ```output
        レシピ数（例：5：2
        材料のリスト（例：鶏肉、ジャガイモ、ニンジン：リンゴ、小麦粉
        フィルター（例：ベジタリアン、ビーガン、グルテンフリー：砂糖

        
        -りんごと小麦粉のパンケーキ： 小麦粉1カップ、ベーキングパウダー小さじ1/2、重曹小さじ1/2、塩小さじ1/4、砂糖大さじ1、卵1個、バターミルクまたはサワーミルク1カップ、溶かしバター1/4カップ、皮をむいてすりおろしたグラニースミスアップル1個。
        -リンゴのフリッター： リンゴのフリッター：小麦粉1～1/2カップ、ベーキングパウダー小さじ1、塩小さじ1/4、重曹小さじ1/4、ナツメグ小さじ1/4、シナモン小さじ1/4、オールスパイス小さじ1/4、砂糖1/4カップ、植物性ショートニング1/4カップ、牛乳1/4カップ、卵1個、皮をむいて細切りにしたリンゴ2カップ
        買い物リスト
        -小麦粉、ベーキングパウダー、重曹、塩、砂糖、卵、バターミルク、バター、リンゴ、ナツメグ、シナモン、オールスパイス 
        ```

## セットアップの改善

ここまでのコードは動作するものだが、さらに改善するための微調整が必要だ。いくつかやるべきことがある：

- **APIキーのように、コードから秘密を分離する。** 秘密はコードに属するものではなく、安全な場所に保管されるべきものです。シークレットをコードから切り離すには、環境変数や`python-dotenv`のようなライブラリを使ってファイルから読み込むことができる。コードでは以下のようになる：

    1. 次のような内容の `.env` ファイルを作成する：

        ```bash
        OPENAI_API_KEY=sk-...
        ```

    > Azureでは、以下の環境変数を設定する必要がある：

        ```bash
        OPENAI_API_TYPE=azure
        OPENAI_API_VERSION=2023-05-15
        OPENAI_API_BASE=<replace>
        ```

        コードでは、次のように環境変数をロードする：

        ```python
        from dotenv import load_dotenv

        load_dotenv()

        openai.api_key = os.environ["OPENAI_API_KEY"]
        ```

- **トークンの長さ**について一言。私たちが望むテキストを生成するのに必要なトークンの数を考えるべきです。トークンにはお金がかかるので、可能な限り、使用するトークンの数を節約するようにすべきです。たとえば、より少ないトークンで済むように、プロンプトにフレーズをつけることはできないだろうか。

   使用するトークンを変更するには、 `max_tokens` パラメータを使用します。例えば、100個のトークンを使いたい場合はこうする：

    ```python
    completion = openai.Completion.create(model="davinci-002", prompt=prompt, max_tokens=100)
    ```

- **温度(temperature)に関する実験**。温度(temperature)についてはこれまで触れてこなかったが、我々のプログラムがどのように機能するかという重要な背景である。温度(temperature)の値が高ければ高いほど、出力はよりランダムになる。逆に温度(temperature)が低ければ低いほど、出力は予測しやすくなる。出力にばらつきが欲しいか欲しくないかを考えてみてください。

   温度(temperature)を変更するには、`temperature`パラメータを使います。例えば、0.5の温度(temperature)を使いたい場合はこうする：

    ```python
    completion = openai.Completion.create(model="davinci-002", prompt=prompt, temperature=0.5)
    ```

   > 1.0に近ければ近いほど、出力は変化する。

## 課題

この課題では、何を作るかを選ぶことができる。

以下はいくつかの提案です：

- レシピジェネレーターアプリをさらに改良する。温度値やプロンプトを弄って、何ができるか試してみましょう。
- "スタディ・バディ"を作る。このアプリは、例えばPythonのようなトピックに関する質問に答えることができるはずだ。"Pythonのあるトピックは何ですか？"というようなプロンプトを用意したり、あるトピックに関するコードを見せてください、といったプロンプトを用意したりできるはずだ。
- 歴史ボット、歴史を生き生きとさせる。ボットに特定の歴史上の人物を演じさせ、その人生や時代について質問する。

## 解決策

### 勉強仲間

以下はスタータープロンプトです。どのように使えるか、好みに合わせて微調整してみてください。

```text
- "あなたはPython言語のエキスパートです

    以下のフォーマットでPythonの初心者向けレッスンを提案してください：

    フォーマット
    - コンセプト
    - レッスンの簡単な説明
    - コードによる演習と解答"
```

### 歴史ボット

以下は使用可能なプロンプトです：

```text
- "あなたはエイブ・リンカーンです、あなた自身について3つの文章で教えてください。"
- "エイブが使ったような文法と単語を使って答えてください。"
- "あなたはエイブ・リンカーンです、エイブが使ったような文法と言葉を使って答えてください"

   あなたの最も偉大な業績について300語で教えてください。
```

## 知識チェック

温度(temperature )は何をする概念か？

1. 出力のランダム性を制御する。
1. 応答の大きさを制御する。
1. トークンの数を制御します。


## 🚀チャレンジ

0が最も変化が少なく、1が最も変化が大きいことを忘れないでください。

## 素晴らしい作品です！学習を続ける

このレッスンを終えたら、[生成AI学習コレクション](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)をチェックして、生成AI知識のレベルアップを続けましょう！

レッスン7では、[チャットアプリケーションの構築](../../../07-building-chat-applications/translations/ja/README.md)の方法を見ていきます！
