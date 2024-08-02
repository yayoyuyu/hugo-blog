+++
title = 'ddc-fuzzyにddc-filter-matcher_prefixを合わせるといい感じかも'
date = 2024-08-03T02:16:01+09:00
draft = false
+++

基本的にddc.vimのfilterはddc-fuzzyを使っていたのだけど、途中の部分にマッチしたものが出ていたのが出てくるのがあまり好きじゃなかったのでmatcher_prefixでprefixにマッチさせたら結構いい感じだったかもしれない。

![ビフォーアフター](https://i.gyazo.com/3343fb764fb0fc88a7d9c3b37bb3da42.webp)

だいたいこんなかんじ。

ちなみにmatcher_prefixのparamsのprefixLengthを変更するとprefixの長さを変更出来る。
