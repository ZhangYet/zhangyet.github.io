---
title: "继续学日语"
tags: ["japanese"]
date: 2019-05-30 12:14:07
layout: post
excerpt: 这不知道是我第几次决心学日语了。
categories: japanese
comments: true
---

5月24日的时候，我在想，要不要考虑专门看点佛教的书，了解一下小乘佛教的资料？后来找了一圈，也没有找到合适的资料。心想其实自己瞎看，意义也不大，最多成为半个历史民科。不如从头学日语吧。

我曾经数次起念学日语。最后五十音图都没有背下来。

希望这次好一点，我还特意写了脚本来生成测试，帮助我背五十音图。

``` python
import random
import sys
import fire

vowel = ['a', 'i', 'u', 'e', 'o']
line_index = ['a', 'k', 's', 't', 'n', 'h', 'm', 'y', 'r', 'w', 'nn']

roma = {
    'a': vowel,
    'k': ['k' + c for c in vowel],
    's': ['s' + c if c != 'i' else 'sh' + c for c in vowel],
    't': ['ta', 'chi', 'tsu', 'te', 'to'],
    'n': ['n' + c for c in vowel],
    'h': ['h' + c if c != 'u' else 'f' + c for c in vowel],
    'm': ['m' + c for c in vowel],
    'y': ['y' + c for c in vowel if c not in ['i', 'e']],
    'r': ['r' + c for c in vowel],
    'w': ['w' + c for c in vowel if c != 'u'],
    'nn': ['n'],
}

hiragana = {
    line_index[0]: ['あ', 'い', 'う', 'え', 'お'],
    line_index[1]: ['か', 'き', 'く', 'け', 'こ'],
    line_index[2]: ['さ', 'し', 'す', 'せ', 'そ'],
    line_index[3]: ['た', 'ち', 'つ', 'て', 'と'],
    line_index[4]: ['な', 'に', 'ぬ', 'ね', 'の'],
    line_index[5]: ['は', 'ひ', 'ふ', 'へ', 'ほ'],
    line_index[6]: ['ま', 'み', 'む', 'め', 'も'],
    line_index[7]: ['や', 'ゆ', 'よ'],
    line_index[8]: ['ら', 'り', 'る', 'れ', 'ろ'],
    line_index[9]: ['わ', 'ゐ', 'ゑ', 'を'],
    line_index[10]: ['ん'],
}

katakana = {
    line_index[0]: ['ア', 'イ', 'ウ', 'エ', 'オ'],
    line_index[1]: ['カ', 'キ', 'ク', 'ケ', 'コ'],
    line_index[2]: ['サ', 'シ', 'ス', 'セ', 'ソ'],
    line_index[3]: ['タ', 'チ', 'ツ', 'テ', 'ト'],
    line_index[4]: ['ナ', 'ニ', 'ヌ', 'ネ', 'ノ'],
    line_index[5]: ['ハ', 'ヒ', 'フ', 'ヘ', 'ホ'],
    line_index[6]: ['マ', 'ミ', 'ム', 'メ', 'モ'],
    line_index[7]: ['ヤ', 'ユ', 'ヨ'],
    line_index[8]: ['ラ', 'リ', 'ル', 'レ', 'ロ'],
    line_index[9]: ['ワ', 'ヰ', 'ヱ', 'ヲ'],
    line_index[10]: ['ン'],
}


class Learner():
    def reference(self, lines):
        print(lines)
        r = []
        h = []
        ki = []
        for k in lines:
            r += roma[k]
            h += hiragana[k]
            ki += katakana[k]

        print('roma:     {}'.format(r))
        print('hiragana: {}'.format(h))
        print('katakana: {}'.format(ki))

    def gen_test(self, typ, lines):
        ret = []
        if typ == 'roma':
            data = roma
        elif typ == 'hira':
            data = hiragana
        else:
            data = katakana
        for k in lines:
            ret += data[k]

        random.shuffle(ret)
        print(ret)


if __name__ == "__main__":
    fire.Fire(Learner)
```
