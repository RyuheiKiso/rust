# unsafe Rust

このセクションでは、Rustの非安全コードについて詳しく説明します。

## unsafeブロック

`unsafe`ブロックを使用すると、Rustの安全性保証を一時的に無効化し、低レベルの操作を実行できます。ただし、これによりプログラムの安全性が損なわれる可能性があるため、慎重に使用する必要があります。

- **基本構文**:
  ```rust
  let mut num = 5;
  let r1 = &num as *const i32; // 不変の生ポインタ
  let r2 = &mut num as *mut i32; // 可変の生ポインタ

  unsafe {
      println!("r1: {}", *r1);
      println!("r2: {}", *r2);
  }
  ```

- **注意点**:
  - 生ポインタの操作は、未定義動作を引き起こす可能性があります。
  - `unsafe`ブロック内でのみ生ポインタを解参照できます。

## 生ポインタ

生ポインタ（raw pointer）は、Rustの所有権や借用ルールを無視してメモリを直接操作するための手段です。

- **特徴**:
  - 不変ポインタ（`*const T`）と可変ポインタ（`*mut T`）の2種類があります。
  - メモリの安全性は保証されません。

- **例**:
  ```rust
  let mut value = 42;
  let ptr = &mut value as *mut i32;

  unsafe {
      *ptr += 1;
      println!("value: {}", *ptr);
  }
  ```

## 外部関数インターフェース（FFI）

FFI（Foreign Function Interface）を使用すると、Rustから他のプログラミング言語（例: C）の関数を呼び出すことができます。

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

- **`extern`ブロック**:
  - 外部関数を宣言します。
  - `"C"`はC言語のABI（Application Binary Interface）を指定します。

- **安全性の考慮**:
  - 外部関数の呼び出しは`unsafe`ブロック内で行う必要があります。

## unsafeのベストプラクティス

- **最小限に留める**:
  - `unsafe`コードは必要最小限に抑え、可能な限り安全なコードでラップします。
- **ドキュメントを追加**:
  - `unsafe`コードの意図や使用方法を明確に記述します。
- **テストを徹底**:
  - `unsafe`コードが正しく動作することを確認するために、十分なテストを行います。

`unsafe`を適切に使用することで、Rustの安全性を維持しながら低レベルの操作を実現できます。
