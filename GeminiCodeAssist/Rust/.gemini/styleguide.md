# Rust スタイルガイド (Google Gemini Code Assist 向け)

このスタイルガイドは、Google Gemini Code Assist を使用して Rust コードを生成する際に、より良い結果を得るための推奨事項をまとめたものです。Gemini Code Assist は文脈を理解し、既存のコードスタイルに合わせようとしますが、このガイドラインに従うことで、より一貫性があり、保守性の高いコードを生成できます。

**注:** これは一般的な Rust スタイルガイドを網羅するものではありません。Rust の公式スタイルガイド ([https://doc.rust-lang.org/style/](https://doc.rust-lang.org/style/)) および [Rustfmt](https://rust-lang.github.io/rustfmt/) の使用を強く推奨します。このガイドは、Gemini Code Assist を使用する際に特に注意すべき点を補足するものです。

## 1. 一般的なコーディングスタイル

* **明示的な指示:** Gemini Code Assist に期待する動作を明確に指示してください。コメントや既存のコードで、意図を明確に伝えましょう。
  * 例:

        ```rust
        // TODO: この関数は、与えられたベクターの要素の合計を計算する。
        // エラーハンドリング: ベクターが空の場合は 0 を返す。
        fn calculate_sum(numbers: &Vec<i32>) -> i32 {
            // ... Gemini Code Assist がここにコードを生成 ...
        }
        ```

* **既存のスタイルに従う:** プロジェクトに既存のコードがある場合は、そのスタイルに従ってください。Gemini Code Assist は、周囲のコードからスタイルを推測します。
* **簡潔なコード:** 短く、焦点を絞った関数やメソッドを作成します。これにより、Gemini Code Assist がより正確なコードを生成しやすくなります。
* **エラーハンドリング:**  エラーハンドリングの方法を明示的に指示します。`Result` 型を適切に使用し、エラーメッセージを明確に記述します。
  * 例：`// エラーハンドリング: ファイルが存在しない場合はエラーを返す`

## 2. Rust 固有の考慮事項

* **所有権と借用:** 所有権、借用、ライフタイムに関するコメントを記述すると、Gemini Code Assist がより適切なコードを生成するのに役立ちます。
  * 例:

        ```rust
        // この関数は、文字列のスライスを受け取り、新しい文字列を返す。
        // 元の文字列は変更されない。
        fn process_string(input: &str) -> String {
          // ...
        }
        ```

* **`Result` と `Option`:**  `Result` と `Option` を適切に使い分け、エラーや値の不在を処理する方法を明確にします。
  * `Result`: 回復可能なエラーを表す場合に使用します。
  * `Option`: 値が存在しない可能性がある場合に使用します。(例: 検索結果が見つからない場合)

* **`unwrap()`、`expect()`、`?` 演算子:**
  * `unwrap()` や `expect()` の使用は最小限に抑え、可能な限り `?` 演算子を使用してエラーを伝播させます。
  * Geminiに指示する場合、`unwrap()` や `expect()` を使うべきか、`?` を使うべきか、あるいは他のエラーハンドリング（matchなど）を行うべきか明確にしましょう。

* **イテレータ:** イテレータを使用する場合は、どのような操作を行うかを明確にコメントで記述します。
  * 例:

        ```rust
        // このベクターの各要素を2倍にして、新しいベクターを作成する。
        let doubled_numbers: Vec<i32> = numbers.iter().map(|x| x * 2).collect();
        ```

* **マクロ:** マクロを使用する場合は、マクロの目的と使用方法をコメントで説明します。

* **unsafe コード:** `unsafe` コードが必要な場合は、その理由と安全性を確保するための対策をコメントで詳しく説明します。 Gemini Code Assist は `unsafe` コードの生成を避ける傾向があるため、`unsafe` が本当に必要であることを明示する必要があります。

* **async/await:** 非同期処理を行う場合は、`async` 関数や `await` 式を使用することを明示的に指示します。

## 3. コメント

* **詳細なコメント:** 関数、構造体、メソッドの目的と動作を説明するコメントを記述します。
* **TODO コメント:** Gemini Code Assist にコードを生成させたい箇所には、`// TODO:` コメントを使用します。
* **インラインコメント:** 必要に応じて、コードの特定の部分の動作を説明するインラインコメントを追加します。

## 4. テスト

* **テストの指示:**  どのようなテストケースを期待するかを Gemini Code Assist に指示します。
  * 例:

        ```rust
        // TODO: この関数のテストを記述する。
        // テストケース:
        // - 空のベクターの場合
        // - 1つの要素を持つベクターの場合
        // - 複数の要素を持つベクターの場合
        // - 負の数を含むベクターの場合
        ```

* **テスト駆動開発 (TDD):**  先にテストを記述し、その後で Gemini Code Assist に実装を生成させることも有効です。

## 5. リファクタリング

* **段階的なリファクタリング:** Gemini Code Assist は、一度に大きな変更を加えるよりも、段階的なリファクタリングの方が得意です。
* **リファクタリング指示:** リファクタリングの意図を明確にコメントで記述してください。
  * 例: `// TODO: この関数をリファクタリングして、可読性を向上させる。`

## 例：完全な関数の生成

```rust
// この関数は、与えられた文字列内の各単語を逆順にする。
// 例: "hello world" -> "olleh dlrow"
// 空白はそのまま保持する。
//
// エラーハンドリング:
// 入力が空文字列の場合は、空文字列を返す
fn reverse_words(input: &str) -> String {
    // TODO: 実装をここに記述
    if input.is_empty() {
        return String::new();
    }

    input
        .split_whitespace()
        .map(|word| word.chars().rev().collect::<String>())
        .collect::<Vec<String>>()
        .join(" ")
}

#[cfg(test)]
mod tests {
    use super::*;

    // TODO: reverse_words 関数のテストを記述する。
    // テストケース：
    // - 空文字列
    // - 1つの単語
    // - 複数の単語（空白を含む）
    // - 特殊文字を含む単語

    #[test]
    fn test_reverse_words_empty() {
        assert_eq!(reverse_words(""), "");
    }

    #[test]
    fn test_reverse_words_single() {
        assert_eq!(reverse_words("hello"), "olleh");
    }

    #[test]
    fn test_reverse_words_multiple() {
        assert_eq!(reverse_words("hello world"), "olleh dlrow");
    }

    #[test]
    fn test_reverse_words_with_special_chars() {
        assert_eq!(reverse_words("hello! world?"), "!olleh ?dlrow");
    }

}
