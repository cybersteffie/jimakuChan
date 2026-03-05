# KNOWLEDGE - ドメイン知識・調査結果

## 業務・ドメイン知識
- jimakuChanは配信者向けリアルタイム音声認識字幕アプリ
- index.html（設定UI）→ URLパラメータ → main.html（iframeで認識エンジン動作）
- Web Speech API（webkitSpeechRecognition）でブラウザ内音声認識
- Google Apps Script経由でGoogle翻訳API利用

## 調査・リサーチ結果
- Chrome v138+でオンデバイス音声認識（processLocally）が利用可能
- Chrome Translation API（Translator/LanguageDetector）で高速ローカル翻訳が可能

## 技術的な知見
- 3層テキストレンダリング（bg/fg/imb）で文字の縁取り効果を実現
- デュアル認識インスタンス方式で音声認識の空白期間を解消
- XOR（key=42）でパスワード難読化

## 決定事項と理由
- 秘密機能（音声認識モデル選択・優先音声認識）はパスワードで隠す → Chrome未実装機能のため一般ユーザー混乱防止
