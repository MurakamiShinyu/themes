<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Step 3. カウンタの表示](#step-3-%E3%82%AB%E3%82%A6%E3%83%B3%E3%82%BF%E3%81%AE%E8%A1%A8%E7%A4%BA)
  - [ページ番号](#%E3%83%9A%E3%83%BC%E3%82%B8%E7%95%AA%E5%8F%B7)
  - [章番号](#%E7%AB%A0%E7%95%AA%E5%8F%B7)
  - [プレビュー](#%E3%83%97%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Step 3. カウンタの表示

次に、ページ番号と章番号を表示してみましょう。SCSS ファイルを作って、そこに新たなスタイルを定義していくことにします。

```bash
$ touch scss/_my_style.scss
```

今作成した SCSS ファイルをメインのスタイルファイル (scss/theme_common.scss) でインポートします。

```scss {highlight: [15]}
// scss/theme_common.scss

// ...
@import '_variables';
@import '_vfm_code';
@import '_vfm_footnotes';
@import '_vfm_frontmatter';
@import '_vfm_hard_new_line';
@import '_vfm_image';
@import '_vfm_math_equation';
@import '_vfm_raw_html';
@import '_vfm_ruby';
@import '_vfm_sectionization';

@import '_my_style';

/* and more... 🖋 */
```

## ページ番号

まずはページ番号です。Vivliostyle は `page` というカウンタ変数を持っているため、通常は自分でページ番号用の変数を定義する必要はありません。このカウンタ変数は自動的に一番最初のページでリセットされ、各ページでインクリメントされるようになっています。ページ番号を表示するには、次のようなスタイルを定義します。

```scss
// scss/theme_common.scss 内で定義済み

// 左ページの場合は左上に表示
@page :left {
  @top-left {
    content: counter(page);
  }
}

// 右ページの場合は右上に表示
@page :right {
  @top-right {
    content: counter(page);
  }
}
```

しかし、カウンタの挙動を変えたい場合（たとえば、目次ではページ番号をインクリメントしたくない場合など）は自分でカウンタ変数を定義する必要があります。このチュートリアルでは目次のページ番号をインクリメントしたくないので、以下のように自分で定義したカウンタ変数 `p` を使うことにします。

```scss {highlight: ['3-25']}
// scss/_my_style.scss

// 一番最初のページでリセット
@page :first {
  counter-reset: p;
}

// 各ページでインクリメント
@page {
  counter-increment: p;
}

// 左ページの場合は左上に表示
@page :left {
  @top-left {
    content: counter(p);
  }
}

// 右ページの場合は右上に表示
@page :right {
  @top-right {
    content: counter(p);
  }
}
```

## 章番号

次に章番号を表示します。`@page :nth(1) {}` を使って、vivliostyle.config.js の `entry` に指定した各原稿ファイルの一番最初のページで、カウンタ変数の `chapter` をインクリメントします。

`@page :first {}` は出版物全体を通して最初のページを指します。一方で `@page :nth(1) {}` は、vivliostyle.config.js の `entry` で指定した原稿ファイルそれぞれの最初のページを指します（これは Vivliostyle 独自の挙動です）。

```scss {highlight: ['3-16']}
// scss/_my_style.scss

// 章番号
@page :nth(1) {
  counter-increment: chapter p;
}

// 章タイトル
section > {
  h1 {
    &::before {
      content: '第 ' counter(chapter) ' 章';
      display: block;
    }
  }
}
```

## プレビュー

これまでの変更をふまえて、プレビューは以下のようになります。オートリロードされない場合はもう一度 `yarn dev` を試してみてください。

![ページ番号と章番号が表示された](/assets/step3.png)
