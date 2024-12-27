+++
title = 'LinuxでVCClientを使ってみる'
date = 2024-12-08T21:00:32+09:00
draft = false
+++

## VCClientとは

[VCClient](https://github.com/w-okada/voice-changer)とは、いわゆるAIボイスチェンジャーと言われるソフト。
RVCやMMVCなど複数のモデルに対応している。  
対応プラットフォームはWindows、M1 Mac、Linux、Google Colab。

## インストール方法について

Windows、M1 Macではビルドされたものが[Hugging Face](https://huggingface.co/wok000/vcclient000/tree/main)でダウンロードできるが、Linuxへは配布されていないので[開発者向けの方法](https://github.com/w-okada/voice-changer/blob/master/README_dev_ja.md)でインストールする。

## インストール

まずはリポジトリをクローンしてcd

```sh
git clone https://github.com/w-okada/voice-changer.git
cd ./voice-changer
```

### 仮想環境をつくる

ドキュメントではAnacondaを使っているが、多分仮想環境を作れるなら何でもよさそうなので
[uv](https://github.com/astral-sh/uv) を使ってみる。
pythonのバージョンは**3.10**なのでオプションで指定しておく。  
仮想環境を作って有効化

```sh
uv venv -p 3.10
source ./.venv/bin/activate
```

仮想環境から抜ける時は

```sh
deactivate
```

でOK

### モジュールをインストールする

[requirements.txt](https://github.com/w-okada/voice-changer/blob/master/server/requirements.txt)からインストール

```sh
uv pip install -r ./server/requirements.txt
```

自分の環境では実行したときに`ModuleNotFoundError`と言われたので追加でインストールした。

```sh
uv pip install fairseq pyworld
```

## つかう

serverディレクトリに入って

```sh
cd ./server
```

起動する。ちなみにこれは公式ドキュメントに書いてあったものからhttpsで起動するオプションを外したもの。

```sh
python3 MMVCServerSIO.py -p 18888 \
    --content_vec_500 pretrain/checkpoint_best_legacy_500.pt  \
    --content_vec_500_onnx pretrain/content_vec_500.onnx \
    --content_vec_500_onnx_on true \
    --hubert_base pretrain/hubert_base.pt \
    --hubert_base_jp pretrain/rinna_hubert_base_jp.pt \
    --hubert_soft pretrain/hubert/hubert-soft-0d54a1f4.pt \
    --nsf_hifigan pretrain/nsf_hifigan/model \
    --crepe_onnx_full pretrain/crepe_onnx_full.onnx \
    --crepe_onnx_tiny pretrain/crepe_onnx_tiny.onnx \
    --rmvpe pretrain/rmvpe.pt \
    --model_dir model_dir \
    --samples samples.json
```

shファイルで保存して実行権限を与えておくとサッと実行できて便利。

初回起動時はモデルのダウンロードが行われる。デフォルト設定ならサーバーが`http://localhost:18888/`で起動するので、適当なブラウザで開く。

チュートリアルは[ここ](https://github.com/w-okada/voice-changer/blob/master/tutorials/tutorial_rvc_ja_latest.md)

### ループバック

実際にボイスチャットなどでマイク入力として使用する場合は、出力から入力へループバックする必要がある。PipeWireなら`libpipewire-module-loopback`を使うとよさそう。
実際の方法は[この記事](https://sorrel.sh/blog/pipewire-how-to-static-loopbacks/)とかを見るのがよさげかも。

### 終了する

CTRL-cで終了できているように見えるが、裏ではまだ生きているのでhtopやらなんやらでkillする。(もっとちゃんとした方法があるかも)  
`deactivate`で仮想環境を抜ければおわり。

## リンク

[VCClient](https://github.com/w-okada/voice-changer)  
[uv](https://github.com/astral-sh/uv)
