+++
title = 'skkeletonがgit経由で取得した辞書を読んでくれない問題の対処'
date = 2023-12-09T02:31:56+09:00
draft = false
+++

## なんでこうなるの
gitが改行コードを自動で変換する設定になっていたせいらしい  
skkeletonは辞書の改行コードがLFじゃないと読んでくれないのでそのせいで失敗してた

## 対処
こんな感じでgitの設定を変更して改行コードの自動変換をオフにすればよい
```
git config --global core.autocrlf false
```
