---
title: 著者詳細ページ
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_detail_page
original_slug: Learn/Server-side/Express_Nodejs/Displaying_data/Author_detail_page
---

著者詳細ページには、指定された `Author` に関する情報を、その (自動的に生成された) `_id` フィールド値を使用して識別し、その `Author` に関連するすべての `Book` オブジェクトのリストを表示する必要があります。

## Controller

Open **/controllers/authorController.js**.

Add the following lines to the top of the file to import the _async_ and _Book_ modules (these are needed for our author detail page).

```js
var async = require("async");
var Book = require("../models/book");
```

Find the exported `author_detail()` controller method and replace it with the following code.

```js
// Display detail page for a specific Author.
exports.author_detail = function (req, res, next) {
  async.parallel(
    {
      author: function (callback) {
        Author.findById(req.params.id).exec(callback);
      },
      authors_books: function (callback) {
        Book.find({ author: req.params.id }, "title summary").exec(callback);
      },
    },
    function (err, results) {
      if (err) {
        return next(err);
      } // Error in API usage.
      if (results.author == null) {
        // No results.
        var err = new Error("Author not found");
        err.status = 404;
        return next(err);
      }
      // Successful, so render.
      res.render("author_detail", {
        title: "Author Detail",
        author: results.author,
        author_books: results.authors_books,
      });
    },
  );
};
```

The method uses `async.parallel()` to query the `Author` and their associated `Book` instances in parallel, with the callback rendering the page when (if) both requests complete successfully. The approach is exactly the same as described for the _Genre detail page_ above.

## View

Create **/views/author_detail.pug** and copy in the following text.

```js
extends layout

block content

  h1 Author: #{author.name}
  p #{author.date_of_birth} - #{author.date_of_death}

  div(style='margin-left:20px;margin-top:20px')

    h4 Books

    dl
      each book in author_books
        dt
          a(href=book.url) #{book.title}
        dd #{book.summary}

      else
        p This author has no books.
```

Everything in this template has been demonstrated in previous sections.

## What does it look like?

Run the application and open your browser to <http://localhost:3000/>. Select the _All Authors_ link, then select one of the authors. If everything is set up correctly, your site should look something like the following screenshot.

![Author Detail Page - Express Local Library site](LocalLibary_Express_Author_Detail.png)

> [!NOTE]
> The appearance of the author _lifespan_ dates is ugly! We'll address that in the final challenge in this article.

## Next steps

- Return to [Express Tutorial Part 5: Displaying library data](/ja/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Proceed to final subarticle of part 5 : [BookInstance detail page and challenge](/ja/docs/Learn/Server-side/Express_Nodejs/Displaying_data/BookInstance_detail_page_and_challenge).
