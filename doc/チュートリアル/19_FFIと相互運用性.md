# FFIと相互運用性

このセクションでは、Rustと他言語の連携について詳しく説明します。

## C言語との連携

Rustは、`extern`キーワードを使用してC言語の関数を呼び出すことができます。これにより、既存のCライブラリをRustプロジェクトで利用可能です。

- **基本構文**:
  ```rust
  extern "C" {
      fn abs(input: i32) -> i32;
  }

  fn main() {
      unsafe {
          println!("abs(-3): {}", abs(-3));
      }
  }
  ```
  - `extern "C"`はC言語のABI（Application Binary Interface）を指定します。
  - `unsafe`ブロック内で外部関数を呼び出します。

- **Cライブラリのリンク**:
  - RustプロジェクトでCライブラリを使用するには、`build.rs`ファイルを作成してビルドプロセスをカスタマイズします。
    ```rust
    println!("cargo:rustc-link-lib=dylib=mylib");
    ```

## 外部ライブラリの利用

Rustでは、`bindgen`ツールを使用してCヘッダファイルをRustのバインディングに変換できます。これにより、Cライブラリを簡単に利用できます。

- **`bindgen`のインストール**:
  ```bash
  cargo install bindgen
  ```

- **バインディングの生成**:
  ```bash
  bindgen input.h -o bindings.rs
  ```

- **生成されたバインディングの使用**:
  ```rust
  include!("bindings.rs");
  ```

## 注意点

FFIを使用する際には、以下の点に注意が必要です。

- **メモリ管理**:
  - RustとCで異なるメモリ管理モデルを使用しているため、メモリの割り当てと解放に注意が必要です。

- **安全性**:
  - 外部関数の呼び出しは`unsafe`ブロック内で行う必要があります。
  - 入力データの検証やエラーハンドリングを適切に行います。

- **ABIの互換性**:
  - RustとCのABIが一致していることを確認します。

FFIを活用することで、Rustのエコシステムを拡張し、既存のライブラリやツールを効率的に利用できます。
