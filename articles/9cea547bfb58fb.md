---
title: "Godot ModdingでMod Uploaderがクラッシュする問題の解決"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["godot", "mod", "modding", "steam"]
published: true
---

## はじめに

今回は`Tiny Pasture`というゲームで再現しましたが、原因から考えれば他のゲームのModdingでも再現する問題です。
そのため、タイトルは`Tiny Pasture`に限定せず問題を特定しやすいと思われる名前に変更しました。

この記事は[Godot Modding 入門](https://zenn.dev/rinjugatla/articles/92907e2c033c2f)に関連して発生した問題の解決記事です。

## 状況

1. Tiny PastureのMODを作成
2. Steam workshopにMODをアップロードするため、betaブランチに切り替え
3. Steamクライアントから(ゲーム本編ではなく)`ゲームエディタを起動`を選択し、起動
4. 即座にクラッシュする

## ログファイル

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

## 原因

ゲーム実行ファイルのフォルダに`override.cfg`が存在するとクラッシュします。
これはGodotゲームの起動時設定を[オーバーライドするために使用するファイル](https://docs.godotengine.org/en/stable/classes/class_projectsettings.html)です。

`override.cfg`を確認するとゲーム本編の設定が書き出されているので、ゲーム本編の起動には影響しません。

Mod Uploaderのような別の設定の実行ファイルを起動したときに本編の設定で上書きしてしまい不整合が起きていました。
本編には含まれるが、Mod Uploaderには含まれないリソースのロードを試みようとしてエラーが大量に発生します。

![alt text](/images/9cea547bfb58fb/tiny-pasture-override_config.png)

:::details 意図せず生成されたoverride.cfg
; Engine configuration file.
; It's best edited using the editor UI and not directly,
; since the parameters that go here are not all obvious.
;
; Format:
;   [section] ; section goes between []
;   param=value ; assign values to parameters

config_version=5

[application]

config/name="TinyPasture"
config/name_localized={
"zh_CN": "动物栏：桌面牧场"
}
run/main_scene="res://Scene/Level/main.tscn"
config/use_custom_user_dir=true
config/features=PackedStringArray("4.3", "GL Compatibility")
run/max_fps=60
boot_splash/bg_color=Color(0.0728426, 0.0728426, 0.0728426, 0)
boot_splash/image="res://logo.png"
boot_splash/fullsize=false
config/icon="res://Assets/UI/PopupUI/Portrait/portrait_rabbit_0_0.png"
config/windows_native_icon="res://Assets/icon.ico"
boot_splash/minimum_display_time=2000

[autoload]

ModLoaderStore="*res://addons/mod_loader/mod_loader_store.gd"
ModLoader="*res://addons/mod_loader/mod_loader.gd"
GState="*res://Manager/GState.gd"
FarmDB="*res://Manager/FarmDB.gd"
GSave="*res://Manager/GSave.gd"
GSteam="*res://Manager/GSteam.gd"
GSound="*res://Manager/GSound.tscn"

[debug]

gdscript/warnings/unused_signal=0
gdscript/warnings/integer_division=0

[display]

window/size/viewport_width=800
window/size/viewport_height=800
window/size/initial_position=Vector2i(960, 0)
window/size/resizable=false
window/size/borderless=true
window/size/transparent=true
window/subwindows/embed_subwindows=false
window/stretch/scale=2.0
window/per_pixel_transparency/allowed=true
window/vsync/vsync_mode=0
mouse_cursor/custom_image="res://Assets/UI/cursor.png"

[editor_plugins]

enabled=PackedStringArray("res://addons/csv_converter/plugin.cfg", "res://addons/mod_loader/_export_plugin/plugin.cfg", "res://addons/mod_tool/plugin.cfg")

[global_group]

FOOD=""
POOP=""
ANIMAL=""
BabyCreater=""
BuildingNode=""

[gui]

theme/custom="res://Scene/UI/default_theme.tres"
theme/custom_font="res://Assets/Fonts/Loaclization/font_en.res"
theme/default_font_subpixel_positioning=0
theme/default_font_multichannel_signed_distance_field=true
theme/default_font_generate_mipmaps=true
timers/tooltip_delay_sec.editor_hint=1.0

[importer_defaults]

font_data_dynamic={
"Compress": null,
"Fallbacks": null,
"Rendering": null,
"allow_system_fallback": true,
"antialiasing": 1,
"compress": true,
"disable_embedded_bitmaps": true,
"fallbacks": [],
"force_autohinter": false,
"generate_mipmaps": true,
"hinting": 0,
"language_support": {},
"msdf_pixel_range": 8,
"msdf_size": 48,
"multichannel_signed_distance_field": true,
"opentype_features": {},
"oversampling": 0.0,
"preload": [],
"script_support": {},
"subpixel_positioning": 1
}

[input]

left_click={
"deadzone": 0.5,
"events": [Object(InputEventMouseButton,"resource_local_to_scene":false,"resource_name":"","device":-1,"window_id":0,"alt_pressed":false,"shift_pressed":false,"ctrl_pressed":false,"meta_pressed":false,"button_mask":1,"position":Vector2(176, 18),"global_position":Vector2(180, 59),"factor":1.0,"button_index":1,"canceled":false,"pressed":true,"double_click":false,"script":null)
]
}
right_click={
"deadzone": 0.5,
"events": [Object(InputEventMouseButton,"resource_local_to_scene":false,"resource_name":"","device":-1,"window_id":0,"alt_pressed":false,"shift_pressed":false,"ctrl_pressed":false,"meta_pressed":false,"button_mask":2,"position":Vector2(219, 16),"global_position":Vector2(223, 57),"factor":1.0,"button_index":2,"canceled":false,"pressed":true,"double_click":false,"script":null)
]
}
escape={
"deadzone": 0.5,
"events": [Object(InputEventKey,"resource_local_to_scene":false,"resource_name":"","device":-1,"window_id":0,"alt_pressed":false,"shift_pressed":false,"ctrl_pressed":false,"meta_pressed":false,"pressed":false,"keycode":0,"physical_keycode":4194305,"key_label":0,"unicode":0,"location":0,"echo":false,"script":null)
]
}
roll_up={
"deadzone": 0.5,
"events": [Object(InputEventMouseButton,"resource_local_to_scene":false,"resource_name":"","device":-1,"window_id":0,"alt_pressed":false,"shift_pressed":false,"ctrl_pressed":false,"meta_pressed":false,"button_mask":8,"position":Vector2(350, 15),"global_position":Vector2(359, 61),"factor":1.0,"button_index":4,"canceled":false,"pressed":true,"double_click":false,"script":null)
]
}
roll_down={
"deadzone": 0.5,
"events": [Object(InputEventMouseButton,"resource_local_to_scene":false,"resource_name":"","device":-1,"window_id":0,"alt_pressed":false,"shift_pressed":false,"ctrl_pressed":false,"meta_pressed":false,"button_mask":16,"position":Vector2(207, 17),"global_position":Vector2(216, 63),"factor":1.0,"button_index":5,"canceled":false,"pressed":true,"double_click":false,"script":null)
]
}
roll_click={
"deadzone": 0.5,
"events": [Object(InputEventMouseButton,"resource_local_to_scene":false,"resource_name":"","device":-1,"window_id":0,"alt_pressed":false,"shift_pressed":false,"ctrl_pressed":false,"meta_pressed":false,"button_mask":4,"position":Vector2(108, 13),"global_position":Vector2(117, 59),"factor":1.0,"button_index":3,"canceled":false,"pressed":true,"double_click":false,"script":null)
]
}
flip_bd={
"deadzone": 0.5,
"events": [Object(InputEventKey,"resource_local_to_scene":false,"resource_name":"","device":-1,"window_id":0,"alt_pressed":false,"shift_pressed":false,"ctrl_pressed":false,"meta_pressed":false,"pressed":false,"keycode":0,"physical_keycode":82,"key_label":0,"unicode":114,"location":0,"echo":false,"script":null)
]
}
boss_key={
"deadzone": 0.5,
"events": [Object(InputEventKey,"resource_local_to_scene":false,"resource_name":"","device":-1,"window_id":0,"alt_pressed":true,"shift_pressed":false,"ctrl_pressed":false,"meta_pressed":false,"pressed":false,"keycode":0,"physical_keycode":96,"key_label":0,"unicode":0,"location":0,"echo":false,"script":null)
]
}

[internationalization]

locale/translation_remaps={
"res://Assets/Fonts/Loaclization/font_en.res": PackedStringArray("res://Assets/Fonts/Loaclization/font_en.res:en", "res://Assets/Fonts/Loaclization/font_jp.res:zh_HK", "res://Assets/Fonts/Loaclization/font_jp.res:ja", "res://Assets/Fonts/Loaclization/font_en.res:zh_CN", "res://Assets/Fonts/Loaclization/font_jp.res:ko"),
"res://Assets/UI/mainMenu/mainMenu_en.png": PackedStringArray("res://Assets/UI/mainMenu/mainMenu_zh.png:zh_CN", "res://Assets/UI/mainMenu/mainMenu_en.png:en", "res://Assets/UI/mainMenu/mainMenu_jp.png:ja", "res://Assets/UI/mainMenu/mainMenu_kr.png:ko", "res://Assets/UI/mainMenu/mainMenu_zh_t.png:zh_HK")
}
locale/translations=PackedStringArray("res://Translation/trans_animal.de.translation", "res://Translation/trans_animal.en.translation", "res://Translation/trans_animal.es.translation", "res://Translation/trans_animal.fr.translation", "res://Translation/trans_animal.ja.translation", "res://Translation/trans_animal.ko.translation", "res://Translation/trans_animal.pl.translation", "res://Translation/trans_animal.pt.translation", "res://Translation/trans_animal.ru.translation", "res://Translation/trans_animal.zh.translation", "res://Translation/trans_animal.zh_HK.translation", "res://Translation/trans_itmes.de.translation", "res://Translation/trans_itmes.en.translation", "res://Translation/trans_itmes.es.translation", "res://Translation/trans_itmes.fr.translation", "res://Translation/trans_itmes.ja.translation", "res://Translation/trans_itmes.ko.translation", "res://Translation/trans_itmes.pl.translation", "res://Translation/trans_itmes.pt.translation", "res://Translation/trans_itmes.ru.translation", "res://Translation/trans_itmes.zh.translation", "res://Translation/trans_itmes.zh_HK.translation", "res://Translation/trans_system.de.translation", "res://Translation/trans_system.en.translation", "res://Translation/trans_system.es.translation", "res://Translation/trans_system.fr.translation", "res://Translation/trans_system.ja.translation", "res://Translation/trans_system.ko.translation", "res://Translation/trans_system.pl.translation", "res://Translation/trans_system.pt.translation", "res://Translation/trans_system.ru.translation", "res://Translation/trans_system.zh.translation", "res://Translation/trans_system.zh_HK.translation", "res://Translation/trans_staff.de.translation", "res://Translation/trans_staff.en.translation", "res://Translation/trans_staff.es.translation", "res://Translation/trans_staff.fr.translation", "res://Translation/trans_staff.ja.translation", "res://Translation/trans_staff.ko.translation", "res://Translation/trans_staff.pl.translation", "res://Translation/trans_staff.pt.translation", "res://Translation/trans_staff.ru.translation", "res://Translation/trans_staff.zh.translation", "res://Translation/trans_staff.zh_HK.translation", "res://Translation/trans_animal.la.translation", "res://Translation/trans_animal.pt_BR.translation", "res://Translation/trans_animal.tr.translation", "res://Translation/trans_itmes.la.translation", "res://Translation/trans_itmes.pt_BR.translation", "res://Translation/trans_itmes.tr.translation", "res://Translation/trans_system.la.translation", "res://Translation/trans_system.pt_BR.translation", "res://Translation/trans_system.tr.translation")
locale/test="en"
locale/language_filter=["de", "en", "es", "fr", "ja", "ko", "la", "pl", "pt", "ru", "tr", "zh"]
locale/locale_filter_mode=1
locale/country_filter=["BR", "CN", "HK"]

[rendering]

textures/canvas_textures/default_texture_filter=0
renderer/rendering_method="gl_compatibility"
renderer/rendering_method.mobile="gl_compatibility"
textures/vram_compression/import_etc2_astc=true
viewport/transparent_background=true
viewport/hdr_2d=true
2d/snap/snap_2d_transforms_to_pixel=true
2d/snap/snap_2d_vertices_to_pixel=true
:::

## 解決

`override.cfg`を削除します。
たったこれだけで解決します。

## 解決のきっかけ

起動できる時と起動できない時のゲームフォルダの差分を取ってはじめて`override.cfg`の存在に気が付きました。
てっきり実行ファイルのハッシュに差分が出ると思っていましたがまさかの結果かつクリティカルな差分でした。

![alt text](/images/9cea547bfb58fb/tiny-pasture-diff.png)

## override.cfgファイルができるタイミング

問題は意図せずこのファイルが生成されてしまい、ゲームをクラッシュさせていたことにあります。
このファイルが生成されるタイミングですが、`ModLoaderMod.register_global_classes_from_array`を誤った書式で使用すると生成されることを確認しました。

- 独自定義のクラス

```gdscript:mod_main.gd
class_name MyClass
extends Node

func _ready() -> void:
    pass
```

- MODのエントリスクリプト

```gdscript:mod_main.gd
extends Node

func _init() -> void :
    # このコードが実行されるとoverride.cfgが生成される
    # このコードはregister_global_classes_from_arrayの書式に沿っていないので誤り
    # 例外処理でModLoaderUtils.register_global_classes_from_arrayの処理をすぐに抜けるので、
    # 後続のoverride.cfg生成処理が走る
    ModLoaderMod.register_global_classes_from_array(["res://mods-unpacked/rin_jugatla-global_class/test_class.gd"])
```

```gdscript:mod_main.gd
extends Node

func _init() -> void :
    # このコードではoverride.cfgは生成されない
    # このコードはregister_global_classes_from_arrayの書式に沿っている
    # そもそもこの書式だと起動直後にクラッシュするのでoverride.cfgの生成コードまで到達しないと思われる
    ModLoaderMod.register_global_classes_from_array(#ModLoaderMod.register_global_classes_from_array([{ 
        "base": "Node", 
        "class": "MyClass", 
        "language": "GDScript", 
        "path": "res://mods-unpacked/rin_jugatla-global_class/test_class.gd" 
    }]))
```

ここで生成されていそう。

![alt text](/images/9cea547bfb58fb/godot-mod-tool-create-override-config.png)
