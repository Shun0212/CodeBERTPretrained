# CodeMorph-BERTの実装から評価

## はじめに

このリポジトリは、**Google Colab** 上で **CodeSearchNet** の Python コードデータセットを使用し、**Masked Language Modeling (MLM)** によって **CodeBERT** モデルをゼロから事前学習するサンプルコードを提供します。
### Hugging Face Hub リポジトリ情報

*   **Hugging Face Hub リンク:**  [CodeMorph-BERT](https://huggingface.co/Shuu12121/CodeMorph-BERT)

---

## セットアップと初期化

以下の手順でセットアップと初期化を行います。

1.  **必要なライブラリのインストール**
2.  **Google Drive のマウント (Google Colab を使用する場合)**
3.  **作業環境の設定**

---

## データ準備

**CodeSearchNet** データセットから Python コードサンプルを抽出し、モデル学習のために必要な前処理を行います。

---

## トークナイザーの学習

**WordPiece** ベースのトークナイザーを学習させます。このトークナイザーは、後続のステップでコードをトークン化するために使用されます。

---

## BERT モデルの初期化

**Masked Language Modeling (MLM)** タスクに適した **BERT** モデルを初期化します。モデルの構成と設定を行います。

---

## データセットのトークナイズ

準備したコードサンプルを、学習済みのトークナイザーを用いてトークン化し、モデルへの入力形式に変換します。

---

## トレーニングプロセス

**Hugging Face Transformers** ライブラリの **Trainer API** を利用してモデルを学習させます。トレーニング中は、チェックポイントの保存と評価を行います。

---

## チェックポイント管理

トレーニングの再開を容易にするため、既存のチェックポイントをロードする機能を提供します。

---

## トレーニング手順

トレーニングの具体的な手順は以下の通りです。

### 1. セットアップ

*   **必要なライブラリをインストールします。**

    ```python
    !pip install datasets transformers
    ```

*   **Google Drive をマウントします (必要に応じて)。**

    ```python
    from google.colab import drive
    drive.mount('/content/drive')
    ```

*   **作業ディレクトリを設定します。**

### 2. データ準備

*   **CodeSearchNet データセットから Python コードデータをロードします。**
*   **`func_code_string` カラムから関数コード文字列を抽出します。**

### 3. トークナイザーの学習

*   **WordPiece トークナイザーを学習させます。**
*   **学習したトークナイザーを保存します。**

### 4. モデルの初期化

*   **BERT の構成を設定します。**
*   **学習済みトークナイザーを使用して BERT モデルを初期化します。**

### 5. データセットのトークナイズ

*   **学習したトークナイザーを使用して、データセットをトークン化します。**
*   **必要に応じて、トランケーションとパディングを適用します。**

### 6. モデルのテスト

*   **Masked Language Modeling (MLM) タスク**
    *   `[MASK]` トークンでマスクした文字列を用いて、マスクされたトークンを予測するタスクを行います。
*   **関数名予測タスク**
    *   関数名を `[MASK]` トークンに置き換え、元の関数名を予測するタスクを行います。

---

## 使用データセット

使用するデータセットの詳細は以下の通りです。

*   **データセット名:** CodeSearchNet
*   **データタイプ:** Python コード
*   **データセットサイズ:** 約 400,000 サンプル (train, validation, test の合計)
*   **主なカラム:**
    *   `func_code_string`: Python 関数コード
    *   `docstring`: 関数の説明 (この実験では未使用)

---



---

## Hugging Face での利用方法

学習済みモデル **CodeMorph-BERT** は、Hugging Face Hub で公開されており、簡単に利用できます。

### モデルのダウンロードとロード

以下のコードでモデルとトークナイザーをロードできます。

```python
from transformers import AutoModelForMaskedLM, AutoTokenizer
import torch

# モデルとトークナイザーのロード
model_name = "Shuu12121/CodeMorph-BERT"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForMaskedLM.from_pretrained(model_name)
```

**詳細:** `Shuu12121/CodeMorph-BERT` モデルは、**Masked Language Modeling** タスクのために事前学習されています。**AutoModelForMaskedLM** と **AutoTokenizer** を使用して、簡単にモデルをロードし、推論に利用できます。

### リポジトリ フォルダ構成

リポジトリの主なフォルダとファイル構成は以下の通りです。

*   **`CodePreBERT.ipynb`**:  CodeBERT モデルを事前学習するための Notebook です。
*   **`EvalCodeMor.ipynb`**:  CodeMorph-BERT と Microsoft CodeBERT の性能を比較評価するための Notebook です。
*   **`LICENSE`**:  リポジトリのライセンス情報。
*   **`README.md`**:  リポジトリの説明や利用方法などを記載した README ファイル。
*   **`UseMyCodeBERT.ipynb`**:  Hugging Face Hub から CodeMorph-BERT を簡単に利用するための Notebook です。
