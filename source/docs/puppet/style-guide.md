---
title: Puppetマニフェストスタイルガイド
---

# manifestコーディング規約

manifestのコーディング規約を決めとこう。

基本的には本家のここに従う。[Style Guide](http://docs.puppetlabs.com/guides/style_guide.html)

いくつか気になるのをピックアップすると。


 * リソースパラメータのデフォルト値はトップレベルスコープにしか置かない(各クラス内で設定しない)。

>9.7. Resource Defaults
>
>Resource defaults should be used in a very controlled manner, and should only be declared at the edges of your manifest ecosystem. Specifically, they may be declared:
>
>    At top scope in site.pp
>        In a class which is guaranteed to never declare another class and never be inherited by another class.
>
>        This is due to the way resource defaults propagate through dynamic scope, which can have unpredictable effects far away from where the default was declared.


 * トップレベルの変数をmodule内で使うときは$::をつける

>11.6. Namespacing Variables
>
>When using top-scope variables, including facts, Puppet modules should explicitly specify the empty namespace (i.e., $::operatingsystem, not $operatingsystem) to prevent accidental scoping issues.


 * 変数にハイフンを使わない

>11.7. Variable format
>
>When defining variables you should only use letters, numbers and underscores. You should specifically not make use of dashes.


 * classのネストや、class内でのdefined resource定義をしない

>11.4. Classes and Defined Resource Types Within Classes
>
>Classes and defined resource types must not be defined within other classes.

- - -

## ペパボ独自規約

### modeのクオート
本家では、ファイルのmodeも 「4桁でクオートすれ」ってあるけど、なんとなく気に食わないので、

```pp
# 3桁でクオート無し
mode => 644
```

を基本とします。

### パラメータ引数とリソース名のクオート

本家では、「変数使うときはダブル、じゃないときはシングル」ってあるけど、エディタでみたときにシンタックスカラーがごっちゃりして解りにくいので、こうします。

```pp
# リソースネームはシングル(変数あるときはダブル)、他は変数含もうが含むまいが全部ダブル。
file { '//foo/baa':
  source => "puppet:///foo/baa",
}
```

### 予約語となっているパラメータ引数はクオートしない

**Good:**

```pp
file { '/tmp':
  ensure => directory,
}
```

**Bad:**

```pp
file { '/tmp':
  ensure => "directory",
}
```


### 同一タイプリソースの間に空行

パラメータが多いと、複数並んだ時に見づらいため、一行空けます。

```pp
file {
  '/foo/baa':
    source  => "puppet///foo/baa",
    mode    => 644,
    require => File['/foo'];

  '/aaa/bbb':
    content => template("aaa/bbb"),
    mode    => 755;
}
```

### 複数requireで改行

横に並べるとあとから編集しにくいので改行いれる。

```pp
sevice { 'mydaemon':
  ensure  => running,
  require => [
    Package['mydaemon'],
    File['mydaemon.conf'],
  ]
}
```

### include は1つずつ

横に並べない。includeの順はそれなりに処理順でもあるので、横並びだと、順番を変えたい時に編集がめんどうです。

```pp
# これダメ
include base, base::config, base::packages
# こう
include base
include base::config
include base::packages
```

### Relationship Declarationsのインデント

縦方向に整列することで読みやすくする。

**Good:**

```pp
   Class['hoge']
-> Class['hoge::install']
```

**Bad:**

```pp
Class['hoge']
-> Class['hoge::install']
```
