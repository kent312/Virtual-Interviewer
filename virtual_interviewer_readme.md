# Virtual Interviewer System

AI搭載 面接練習支援システム - あなたの面接スキルを向上させる

## 概要

Virtual Interviewer Systemは、実際の面接を想定した練習環境を提供するStreamlitベースのWebアプリケーションです。OpenAI GPTを活用して、リアルタイムで回答を評価し、具体的な改善提案を行います。職種別・企業別にカスタマイズされた質問と、STAR法に基づいた構造的な評価により、効果的な面接対策が可能です。

## 機能

- **職種別質問データベース**: IT、営業、管理職など、職種に応じた質問セット
- **リアルタイム評価**: GPT-4による即時フィードバックと改善提案
- **STAR法準拠評価**: Situation, Task, Action, Resultの観点から構造的に評価
- **音声入力対応**: Web Speech APIを使用した音声での回答入力（Phase 2）
- **進捗トラッキング**: 練習履歴と成長の可視化
- **模擬圧迫面接**: ストレス耐性を鍛える高難度モード
- **企業別カスタマイズ**: 特定企業の面接傾向を反映した質問生成

## 入力

### テキスト入力
- **形式**: プレーンテキスト
- **文字数制限**: 2000文字（推奨: 200-500文字）
- **言語**: 日本語、英語（自動検出）

### 音声入力（Phase 2）
- **形式**: Web Speech API経由のリアルタイム音声認識
- **対応ブラウザ**: Chrome, Edge（最新版推奨）
- **言語**: 日本語、英語

### 設定ファイル
- **ファイル**: `config/interview_settings.json`
- **内容**: 質問データベース、評価基準、企業プロファイル
- **環境変数**: `.env`ファイルでAPIキー管理

## 出力

### 評価レポート
- **形式**: JSON形式の評価データ
- **内容**: 
  - 総合スコア（100点満点）
  - カテゴリ別評価（5段階）
  - 具体的な改善提案
  - 模範回答例

### ログファイル
- **ファイル**: `logs/interview_history.csv`
- **内容**: 日時、質問、回答、評価スコア、フィードバック
- **用途**: 進捗分析、弱点特定

### PDFレポート（Phase 3）
- **ファイル**: `reports/interview_report_YYYYMMDD.pdf`
- **内容**: 面接練習の総合分析レポート

## 必要な環境

### システム要件
- **OS**: Windows 10/11, macOS 10.15+, Ubuntu 20.04+
- **Python**: 3.8以上（推奨: 3.9+）
- **メモリ**: 4GB以上（推奨: 8GB+）
- **ブラウザ**: Chrome/Edge/Firefox最新版
- **ネットワーク**: 安定したインターネット接続

### 必須ソフトウェア
```bash
# Python（システムにインストール済みであること）
python --version  # 3.8以上を確認

# Git（オプション、開発時に推奨）
git --version
```

## 依存関係

### コアライブラリ
```
streamlit==1.28.0          # Webアプリケーションフレームワーク
openai==1.3.0              # OpenAI GPT API
python-dotenv==1.0.0       # 環境変数管理
```

### データ処理・分析
```
pandas==2.1.0              # データ分析・管理
numpy==1.24.3              # 数値計算
matplotlib==3.7.2          # グラフ描画
seaborn==0.12.2           # 統計グラフ
```

### 音声処理（Phase 2）
```
streamlit-webrtc==0.45.0   # WebRTC統合
speechrecognition==3.10.0  # 音声認識
pydub==0.25.1             # 音声ファイル処理
```

### PDF生成（Phase 3）
```
reportlab==4.0.4           # PDFレポート生成
Pillow==10.0.0            # 画像処理
```

### 開発・テスト用
```
pytest==7.4.0              # テストフレームワーク
black==23.7.0              # コードフォーマッター
flake8==6.1.0             # リンター
pre-commit==3.3.3         # Git hooks
```

## セットアップ

### 1. リポジトリのクローン

```bash
git clone https://github.com/your-org/virtual-interviewer.git
cd virtual-interviewer
```

### 2. Python仮想環境の作成

```bash
# 仮想環境作成
python -m venv venv

# 仮想環境の有効化
# Windows
venv\Scripts\activate

# macOS/Linux
source venv/bin/activate
```

### 3. 依存関係のインストール

```bash
# 本番用パッケージ
pip install -r requirements.txt

# 開発用パッケージ（オプション）
pip install -r requirements-dev.txt
```

### 4. 環境変数の設定

`.env`ファイルを作成し、以下の内容を記述：

```bash
# OpenAI API設定
OPENAI_API_KEY=your_openai_api_key_here
OPENAI_MODEL=gpt-4-turbo-preview  # または gpt-3.5-turbo

# アプリケーション設定
DEBUG_MODE=False
LOG_LEVEL=INFO
MAX_QUESTIONS_PER_SESSION=10
SESSION_TIMEOUT_MINUTES=60

# データベース設定（Phase 4で使用）
DATABASE_URL=sqlite:///interview_data.db
```

### 5. 初期データの準備

```bash
# 質問データベースの初期化
python scripts/init_questions.py

# サンプルデータの確認
python scripts/check_setup.py
```

## 使用方法

### 1. アプリケーションの起動

```bash
# 仮想環境を有効化（まだの場合）
source venv/bin/activate  # Linux/macOS
# または
venv\Scripts\activate     # Windows

# Streamlitアプリ起動
streamlit run app.py

# カスタムポートで起動
streamlit run app.py --server.port 8080
```

### 2. ブラウザでアクセス

```
デフォルト: http://localhost:8501
カスタム: http://localhost:8080
```

### 3. 面接練習の流れ

1. **初期設定**
   - OpenAI APIキーを入力（初回のみ）
   - 職種・難易度を選択
   - 練習モードを選択（通常/圧迫/技術面接）

2. **面接実施**
   - 質問が表示される
   - 回答を入力（テキストまたは音声）
   - 「回答を送信」をクリック

3. **フィードバック確認**
   - 総合スコアを確認
   - 良かった点・改善点を確認
   - 模範回答例を参考にする

4. **結果分析**
   - セッション終了後、総合レポートを確認
   - 苦手分野を特定
   - 次回の練習計画を立てる

## プロジェクト構造

```
virtual-interviewer/
├── app.py                      # メインアプリケーション
├── requirements.txt            # 本番用依存関係
├── requirements-dev.txt        # 開発用依存関係
├── .env.example               # 環境変数テンプレート
├── .gitignore                 # Git除外設定
├── README.md                  # このファイル
├── config/
│   ├── interview_settings.json # 面接設定
│   ├── questions_db.json      # 質問データベース
│   └── companies.json         # 企業プロファイル
├── src/
│   ├── __init__.py
│   ├── core/
│   │   ├── evaluator.py       # 回答評価エンジン
│   │   ├── question_manager.py # 質問管理
│   │   └── session_manager.py  # セッション管理
│   ├── models/
│   │   ├── interview.py       # 面接データモデル
│   │   ├── evaluation.py      # 評価データモデル
│   │   └── user.py           # ユーザーデータモデル
│   ├── ui/
│   │   ├── components.py      # UI コンポーネント
│   │   ├── pages.py          # ページ定義
│   │   └── styles.py         # スタイル定義
│   ├── api/
│   │   ├── openai_client.py   # OpenAI API クライアント
│   │   └── speech_client.py   # 音声認識クライアント
│   └── utils/
│       ├── logger.py          # ログ管理
│       ├── validators.py      # 入力検証
│       └── helpers.py         # ユーティリティ関数
├── data/
│   ├── questions/             # 質問テンプレート
│   ├── feedback/              # フィードバックテンプレート
│   └── reports/               # 生成されたレポート
├── logs/
│   ├── app.log               # アプリケーションログ
│   └── interview_history.csv  # 面接履歴
├── tests/
│   ├── __init__.py
│   ├── test_evaluator.py      # 評価エンジンテスト
│   ├── test_api.py           # API統合テスト
│   └── test_ui.py            # UIテスト
├── scripts/
│   ├── init_questions.py      # 質問DB初期化
│   ├── analyze_results.py     # 結果分析スクリプト
│   └── check_setup.py        # セットアップ確認
└── docs/
    ├── API.md                # API仕様書
    ├── CONTRIBUTING.md       # 貢献ガイド
    └── USER_GUIDE.md         # ユーザーガイド
```

## 開発・デバッグ

### コーディング規約

```bash
# コードフォーマット（Black使用）
black src/ tests/

# リンティング（Flake8使用）
flake8 src/ tests/

# 型チェック（mypy使用）
mypy src/
```

### テストの実行

```bash
# 全テスト実行
pytest

# 特定のテストファイル実行
pytest tests/test_evaluator.py

# カバレッジ付きテスト
pytest --cov=src --cov-report=html

# 継続的テスト（ファイル変更を監視）
ptw
```

### デバッグモード

```python
# .envファイルで設定
DEBUG_MODE=True
LOG_LEVEL=DEBUG

# またはコマンドライン引数
streamlit run app.py --logger.level=debug
```

### パフォーマンス監視

```python
# 処理時間の目標値
- API応答時間: < 2秒
- 評価処理時間: < 3秒
- ページ読み込み: < 1秒
```

## トラブルシューティング

### OpenAI APIエラー

1. **認証エラー（401）**
   ```bash
   # APIキーの確認
   echo $OPENAI_API_KEY
   
   # .envファイルの確認
   cat .env | grep OPENAI_API_KEY
   ```

2. **レート制限エラー（429）**
   ```python
   # 設定でリトライを有効化
   OPENAI_RETRY_ATTEMPTS=3
   OPENAI_RETRY_DELAY=5
   ```

### Streamlitエラー

1. **ポート使用中エラー**
   ```bash
   # 別のポートを指定
   streamlit run app.py --server.port 8502
   
   # プロセスを確認して終了
   lsof -i :8501  # macOS/Linux
   netstat -ano | findstr :8501  # Windows
   ```

2. **セッションエラー**
   ```python
   # セッション状態のリセット
   st.session_state.clear()
   ```

### メモリ不足

```bash
# メモリ使用量の確認
python -m memory_profiler app.py

# ガベージコレクションの強制実行
import gc
gc.collect()
```

## API使用料金の目安

### OpenAI GPT
- **GPT-3.5-turbo**: $0.0015/1Kトークン（入力）、$0.002/1Kトークン（出力）
- **GPT-4**: $0.03/1Kトークン（入力）、$0.06/1Kトークン（出力）
- **GPT-4-turbo**: $0.01/1Kトークン（入力）、$0.03/1Kトークン（出力）

**月額予算例**: 
- 軽度使用（100回/月）: 約$5-10
- 中度使用（500回/月）: 約$25-50
- 重度使用（2000回/月）: 約$100-200

## セキュリティ考慮事項

### APIキー管理
- **絶対に** APIキーをソースコードにハードコーディングしない
- `.env`ファイルは`.gitignore`に含める
- 本番環境では環境変数を使用

### データプライバシー
- ユーザーの回答データは暗号化して保存
- 個人情報は最小限の収集に留める
- データ保持期間を明確に定義（デフォルト: 90日）

### セッション管理
- セッションタイムアウトを設定（デフォルト: 60分）
- クロスサイトスクリプティング（XSS）対策を実装
- 適切な入力検証を実施

## ライセンス

このプロジェクトはMITライセンスの下で公開されています。
詳細は[LICENSE](LICENSE)ファイルを参照してください。

## 貢献

プルリクエストを歓迎します！貢献する前に[CONTRIBUTING.md](docs/CONTRIBUTING.md)をお読みください。

### 開発フロー
1. Issueを作成して機能や修正を提案
2. フォークしてfeatureブランチを作成
3. 変更を実装してテストを追加
4. プルリクエストを作成

## サポート

- **バグ報告**: [GitHub Issues](https://github.com/your-org/virtual-interviewer/issues)
- **機能要望**: [GitHub Discussions](https://github.com/your-org/virtual-interviewer/discussions)
- **ドキュメント**: [Wiki](https://github.com/your-org/virtual-interviewer/wiki)
- **コミュニティ**: [Discord](https://discord.gg/virtual-interviewer)

---

**最終更新**: 2025年7月14日  
**バージョン**: 1.0.0  
**作成者**: Virtual Interviewer開発チーム  
**メンテナー**: [@your-username](https://github.com/your-username)
