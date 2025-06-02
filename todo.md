###今回の修正内容

今回の修正で対象となるエントリポイントはsd-scripts\train\run.batです
これを実行するのに呼び出されないものは今回は対象外です

run.batの中でsdxl_train_network.pyで学習を行い、設定ファイルtrain\config.toml train\dataset.tomlを使い学習します
dataset.tomlの中のimage_dir = 'C:\Users\Owner\Documents\python\sd-scripts\train\img'が学習対象の画像およびタグファイルになります
画像は*_neg.webpと*_pos.webpの二つのタイプが存在ます。

これらのファイルを用いて学習する際、*_pos.webpは通常の学習を行い、*_neg.webpはマイナスの方向に学習を行うように修正します

学習の順番は*の部分が同一の*_neg.webpの画像、*_neg.webpの画像、次の*の部分の画像となります

つまり、このような順番で学習します
8d4bf3a967f15dde13348051365016709168_pos.webp
8d4bf3a967f15dde13348051365016709168_neg.webp
26fd905845e82b8f09814934906896276162_pos.webp
26fd905845e82b8f09814934906896276162_neg.webp
72b24bd5557f521012347951177030584715_pos.webp
72b24bd5557f521012347951177030584715_neg.webp

negもposもついてない画像ファイルが存在する場合、通常の学習として対処します

画像読み込みは現状ではwebpのみを取り扱ってますが、将来的に他の拡張子の画像ファイルが入り込む可能性があります。現状の実装でpngやjpeg等も読み込み可能なはずですので、それらを維持してください
つまりドキュメントでwebpのみを例に取るのは構いませんが、webpだけを走査する実装ではダメです。




### todo.md への追記案

#### データセットクラスの改修 (`sd-scripts\library\dataset.py` 想定)

### 完了

1.  **`ImageInfo` (またはそれに類する内部データ構造) の拡張:**
    *   `DreamBoothDataset` 内で各画像の詳細情報を保持するために使われているであろう `ImageInfo` クラス（または同様の辞書構造）に、画像の種類を示す属性（例: `image_type`）を追加します。
    *   この属性は、`"pos"`、`"neg"`、`"normal"` のような値を持ちます。

    * neg posの判定を追加した


学習を実行するtrain_network.pyの1014行目あたりで判定を行いマイナス方向に学習を行いたいが、ImageInfoにアクセスできない
batchにimageInfoを追加したいが、どうすればいいか
