# 音声認識モデル選択（クラウド/ローカル）ラジオボタン実装計画

## 概要
Chrome v138以降で利用可能なオンデバイス（ローカル）音声認識と、従来のGoogleクラウド音声認識を、ラジオボタンで切り替えられるようにする。

## 現状の理解
- 現在、`main.html` では `processLocally` は「優先音声認識（Word Boost）が設定されている場合のみ」自動的に `true` にされる
- ユーザーが明示的にクラウド/ローカルを選択する手段がない
- `webkitSpeechRecognition.available()` でオンデバイスモデルの利用可否チェックする仕組みは既に `index.html` と `main.html` の両方に存在する

## 変更ファイルと内容

### 1. `index.html` — 設定UI追加
**場所**: 音声認識言語セレクトボックス（行300付近）の直前に、ラジオボタンを追加

```html
<div style="background-color: #e8f0fe; padding: 8px; border-radius: 5px; margin-bottom: 5px;">
  <strong data-i18n="recognitionModel">音声認識モデル：</strong>
  <label>
    <input type="radio" name="recognitionModel" value="cloud" id="recogModelCloud"
           onchange="switchRecognitionModel()" checked>
    <span data-i18n="recogModelCloud">クラウド（Google）</span>
  </label>
  <label>
    <input type="radio" name="recognitionModel" value="local" id="recogModelLocal"
           onchange="switchRecognitionModel()">
    <span data-i18n="recogModelLocal">ローカル（オンデバイス）</span>
  </label>
  <span id="recogModelStatus" style="font-size: 12px; margin-left: 8px;"></span>
</div>
```

**JS追加内容**:
- `switchRecognitionModel()`: ラジオボタン変更時にpostMessageでmain.htmlに通知 + localStorage保存 + updateOptionValues呼び出し
- `saveRecognitionModelToStorage()` / `loadRecognitionModelFromStorage()`: localStorage永続化
- `checkOnDeviceModelAvailability()`: ローカル選択時にモデルの利用可否をステータス表示（既存の `word_boost_status` のチェックコードと同様のロジック）
- ローカルモデルが利用不可（未ダウンロードなど）の場合、ステータスにダウンロードボタンを表示
- URLパラメータに `recog_model=cloud|local` を追加（`updateOptionValues` 内）

### 2. `main.html` — 認識エンジン切り替え
**変更内容**:
- URLパラメータ `recog_model` を受け取る（`getParam('recog_model')`）
- `postMessage` で `updateRecognitionModel` メッセージを受け取る
- `setupRecognitionInstance()` 内で、`recog_model === 'local'` の場合 `rec.processLocally = true` を設定（既存のWord Boost条件とは独立して）
- `initRecognitionInstances()` 内のオンデバイス可否チェックを、Word Boost有無に関係なく `recog_model === 'local'` の場合にも実行
- エラー時のフォールバック処理を調整（ローカルモデルが使えない場合のクラウドへのフォールバック）

### 3. `js/i18n.js` — 翻訳キー追加
```
// 日本語
recognitionModel: "音声認識モデル：",
recogModelCloud: "クラウド（Google）",
recogModelLocal: "ローカル（オンデバイス）",
recogModelStatusAvailable: "✅ 利用可能",
recogModelStatusDownloadable: "📥 要ダウンロード",
recogModelStatusUnavailable: "⚠ 利用不可",
recogModelStatusChecking: "🔍 確認中...",
recogModelDownloadBtn: "モデルをダウンロード",

// 英語
recognitionModel: "Recognition Model:",
recogModelCloud: "Cloud (Google)",
recogModelLocal: "Local (On-device)",
recogModelStatusAvailable: "✅ Available",
recogModelStatusDownloadable: "📥 Download required",
recogModelStatusUnavailable: "⚠ Unavailable",
recogModelStatusChecking: "🔍 Checking...",
recogModelDownloadBtn: "Download Model",
```

### 4. プリセット対応
- `config_values` 配列に `"recog_model"` は含めない（URLパラメータ経由のため別管理）
- プリセット保存(`autoSavePreset`)に `recog_model` を追加
- プリセット読み込み時に `recog_model` のラジオボタンを復元

## 設計ポイント
- 翻訳方法のラジオボタン（`translationMethod`）の実装パターンを踏襲
- ローカルモデルが利用不可のブラウザ（Chrome 137以前）ではローカル選択時に警告を表示
- Word Boostとの関係: ローカルモデル選択時はWord Boostが自動的に有効になり得る。クラウド選択時はWord Boostは無効化される旨を表示
