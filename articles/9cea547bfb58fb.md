---
title: "Tiny PastureのSteam workshop uploaderがクラッシュする問題の解決"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["godot", "steam"]
published: true
---

## 結論

結論から言うと、よくわからないけどバグっていて、よくわからないけど直りました。
いろいろおかしかったので、レアケースとして記録を残すためにこの記事を共有します。

この記事は[Godot Modding 入門](https://zenn.dev/rinjugatla/articles/92907e2c033c2f)に関連して発生した問題の解決記事です。

## Tiny Pasture

ゲーム配信プラットフォームのSteamで配信されている[デスクトップ小動物牧場 - Tiny Pasture](https://store.steampowered.com/app/3167550/_/)というゲームです。

## Tiny Pasture Godot Workshop Utility

Tiny Pastureはユーザの改造(MOD)をSteamで共有する機能があります。
MODをアップロードする際には公式から提供されている`ゲームエディタ`というアプリを利用します。

このアプリはSteamのブランチ機能でbetaブランチとして提供されることが多いです。

### Godot Workshop Utility雛形

GodotでSteam workshopを利用する場合は[Godot Workshop Utility](https://github.com/Blobfish-Games/godot-workshop-utility/)をカスタマイズするようです。

## 状況

1. Tiny PastureのMODを作成
2. Steam workshopにMODをアップロードするため、betaブランチに切り替え
3. Steamクライアントから`ゲームエディタを起動`を選択し、ゲームを起動
4. 即座にクラッシュする

ゲームのログは以下に記録されます。

```text
C:\Users\%username%\AppData\Roaming\TinyPasture\logs
```

:::details ゲームエディタを起動させた際のログ(godot.log)
Godot Engine v4.3.stable.official.77dcf97d8 - <https://godotengine.org>
OpenGL API 3.3.0 NVIDIA 576.02 - Compatibility - Using Device: NVIDIA - NVIDIA GeForce RTX 3080

USER ERROR: Error opening file 'res://logo.png'.
   at: load_image (core/io/image_loader.cpp:90)
USER ERROR: Non-existing or invalid boot splash at 'res://logo.png'. Loading default splash.
   at: setup_boot_logo (main/main.cpp:3185)
USER ERROR: Cannot open file 'res://Translation/trans_animal.de.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.de.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.en.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.en.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.es.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.es.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.fr.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.fr.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.ja.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.ja.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.ko.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.ko.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.pl.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.pl.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.pt.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.pt.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.ru.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.ru.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.zh.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.zh.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.zh_HK.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.zh_HK.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.de.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.de.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.en.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.en.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.es.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.es.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.fr.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.fr.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.ja.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.ja.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.ko.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.ko.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.pl.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.pl.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.pt.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.pt.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.ru.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.ru.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.zh.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.zh.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.zh_HK.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.zh_HK.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.de.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.de.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.en.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.en.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.es.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.es.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.fr.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.fr.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.ja.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.ja.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.ko.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.ko.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.pl.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.pl.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.pt.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.pt.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.ru.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.ru.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.zh.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.zh.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.zh_HK.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.zh_HK.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_staff.de.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_staff.de.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_staff.en.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_staff.en.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_staff.es.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_staff.es.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_staff.fr.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_staff.fr.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_staff.ja.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_staff.ja.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_staff.ko.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_staff.ko.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_staff.pl.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_staff.pl.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_staff.pt.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_staff.pt.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_staff.ru.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_staff.ru.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_staff.zh.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_staff.zh.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_staff.zh_HK.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_staff.zh_HK.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.la.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.la.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.pt_BR.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.pt_BR.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_animal.tr.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_animal.tr.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.la.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.la.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.pt_BR.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.pt_BR.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_itmes.tr.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_itmes.tr.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.la.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.la.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.pt_BR.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.pt_BR.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Cannot open file 'res://Translation/trans_system.tr.translation'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Translation/trans_system.tr.translation. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: No loader found for resource: res://Assets/UI/cursor.png (expected type: )
   at: _load (core/io/resource_loader.cpp:291)
USER ERROR: Cannot open file 'res://Scene/UI/default_theme.tres'.
   at: load (scene/resources/resource_format_text.cpp:1367)
USER ERROR: Failed loading resource: res://Scene/UI/default_theme.tres. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Error loading custom project theme 'res://Scene/UI/default_theme.tres'
   at: initialize_theme (scene/theme/theme_db.cpp:72)
USER WARNING: Translation remap 'res://Assets/Fonts/Loaclization/font_en.res' does not exist. Falling back to 'res://Assets/Fonts/Loaclization/font_en.res'.
   at:_path_remap (core/io/resource_loader.cpp:1054)
USER ERROR: Cannot open file 'res://Assets/Fonts/Loaclization/font_en.res'.
   at: load (core/io/resource_format_binary.cpp:1214)
USER ERROR: Failed loading resource: res://Assets/Fonts/Loaclization/font_en.res. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Error loading custom project font 'res://Assets/Fonts/Loaclization/font_en.res'
   at: initialize_theme (scene/theme/theme_db.cpp:82)
USER ERROR: Attempt to open script 'res://addons/mod_loader/mod_loader_store.gd' resulted in error 'File not found'.
   at: load_source_code (modules/gdscript/gdscript.cpp:1094)
USER ERROR: Failed loading resource: res://addons/mod_loader/mod_loader_store.gd. Make sure resources have been imported by opening the project in the editor at least once.
   at:_load (core/io/resource_loader.cpp:283)
USER ERROR: Failed to instantiate an autoload, can't load from path: res://addons/mod_loader/mod_loader_store.gd.
   at: start (main/main.cpp:3672)
USER ERROR: Attempt to open script 'res://addons/mod_loader/mod_loader.gd' resulted in error 'File not found'.
   at: load_source_code (modules/gdscript/gdscript.cpp:1094)
USER ERROR: Failed loading resource: res://addons/mod_loader/mod_loader.gd. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Failed to instantiate an autoload, can't load from path: res://addons/mod_loader/mod_loader.gd.
   at: start (main/main.cpp:3672)
USER ERROR: Attempt to open script 'res://Manager/GState.gd' resulted in error 'File not found'.
   at: load_source_code (modules/gdscript/gdscript.cpp:1094)
USER ERROR: Failed loading resource: res://Manager/GState.gd. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Failed to instantiate an autoload, can't load from path: res://Manager/GState.gd.
   at: start (main/main.cpp:3672)
USER ERROR: Attempt to open script 'res://Manager/FarmDB.gd' resulted in error 'File not found'.
   at: load_source_code (modules/gdscript/gdscript.cpp:1094)
USER ERROR: Failed loading resource: res://Manager/FarmDB.gd. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Failed to instantiate an autoload, can't load from path: res://Manager/FarmDB.gd.
   at: start (main/main.cpp:3672)
USER ERROR: Attempt to open script 'res://Manager/GSave.gd' resulted in error 'File not found'.
   at: load_source_code (modules/gdscript/gdscript.cpp:1094)
USER ERROR: Failed loading resource: res://Manager/GSave.gd. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Failed to instantiate an autoload, can't load from path: res://Manager/GSave.gd.
   at: start (main/main.cpp:3672)
USER ERROR: Attempt to open script 'res://Manager/GSteam.gd' resulted in error 'File not found'.
   at: load_source_code (modules/gdscript/gdscript.cpp:1094)
USER ERROR: Failed loading resource: res://Manager/GSteam.gd. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Failed to instantiate an autoload, can't load from path: res://Manager/GSteam.gd.
   at: start (main/main.cpp:3672)
USER ERROR: Cannot open file 'res://Manager/GSound.tscn'.
   at: load (scene/resources/resource_format_text.cpp:1367)
USER ERROR: Failed loading resource: res://Manager/GSound.tscn. Make sure resources have been imported by opening the project in the editor at least once.
   at: _load (core/io/resource_loader.cpp:283)
USER ERROR: Failed to instantiate scene state of "res://Manager/GSound.tscn", node count is 0. Make sure the PackedScene resource is valid.
   at: instantiate (scene/resources/packed_scene.cpp:142)
USER ERROR: Failed to instantiate an autoload, path is not pointing to a scene or a script: res://Manager/GSound.tscn.
   at: start (main/main.cpp:3688)
USER ERROR: Cannot open file 'res://Scene/Level/main.tscn'.
   at: load (scene/resources/resource_format_text.cpp:1367)
USER ERROR: Failed loading resource: res://Scene/Level/main.tscn. Make sure resources have been imported by opening the project in the editor at least once.
   at:_load (core/io/resource_loader.cpp:283)
USER ERROR: Failed loading scene: res://Scene/Level/main.tscn.
   at: start (main/main.cpp:3897)
:::

## 解決までに試したこと

時系列順です。

### ブランチの切り替え

通常ブランチとbetaブランチを何度か行ったり来たりしました。

`ゲームエディタ`を起動、クラッシュしました。
解決せず。

### 整合性の確認

Steamでゲームが何か変な時にとりあえず実行するのは整合性の確認です。
ゲームデータ配信サーバに登録されている正常なデータとローカルのデータを比較し、壊れたファイルや足りないファイルをダウンロードしなおします。

`ゲームエディタ`を起動、クラッシュしました。
解決せず。

### Steamクライアントの言語の変更

Steam workshopを見ると昨日も今日も他の人はMODを更新しています。
そして、更新している人は皆中国語を利用しています。

Steamクライアントの表示言語を中国語(簡単な方と難しい方どちらも)に変更しました。
言語を変更するとSteamクライアントが再起動されます。

`ゲームエディタ`を起動、クラッシュしました。
解決せず。

解決しなかったので日本語に戻しました。

### 言語によって配信されるファイルが異なる？

経験はないですが、もしかするとSteamクライアントの言語によって配信されるファイルが異なると考えました。
クライアントの言語を中国語に変更し、整合性の確認を行います。

`ゲームエディタ`を起動、正常に起動しました。
解決しました。

この後、日本語に戻し、整合性を確認し`ゲームエディタ`を起動すると正常に起動しました。
問題が再現しなくなったので言語の問題ではなさそうです。

### 何が問題だった？

結局わからずじまいでしたが、サーバから取得したファイルが古かったのでしょうか？
何が効果的だったのかわかりませんが、同じような状況になったら上の手順とPC再起動とかしてみるといいかもしれません。

SteamCMDでは別ブランチを参照すると変なキャッシュが溜まり、整合性の確認やゲームのアップデートがうまくいかない場合があるので、今回もキャッシュ関係だったのかもしれません。

## そもそも動作がおかしかった

解決後にいろいろ調べたところ`ゲームエディタ`は実行時に`godot.log`を出力しません。
起動時に`godot.log`が出力されていたこと自体がおかしいです。

ただ、これは解決後にわかったなので最初から動作しなかった自分には知る由もありませんでした。

結局何が効果的だったのかわかりませんが、直ってよかった！
