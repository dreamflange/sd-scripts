### 実装済み
エントリーポイントrun.batの中でsdxl_train_network.pyで学習を行い、設定ファイルtrain\config.toml train\dataset.tomlを使い学習します
dataset.tomlの中のimage_dir = 'C:\Users\Owner\Documents\python\sd-scripts\train\img'が学習対象の画像およびタグファイルになります
画像は*_neg.webpと*_pos.webpの二つのタイプが存在ます。

これらのファイルを用いて学習する際、画像ファイル*_pos.<ext>は通常の学習を行い、*_neg.<ext>はマイナスの方向に学習を行うように実装
train_network.py
```
                    # negは反転させる
                    if batch["image_type"]== "neg":
                        loss = -loss
```

###今回の修正内容

学習の順番は*の部分が同一の*_neg.webpの画像、*_neg.webpの画像、次の*の部分の画像となります

つまり、このような順番で学習します
8d4bf3a967f15dde13348051365016709168_pos.webp
8d4bf3a967f15dde13348051365016709168_neg.webp
26fd905845e82b8f09814934906896276162_pos.webp
26fd905845e82b8f09814934906896276162_neg.webp
72b24bd5557f521012347951177030584715_pos.webp
72b24bd5557f521012347951177030584715_neg.webp

negもposもついてない画像ファイルが存在する場合、通常の学習として対処します
