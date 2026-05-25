## Week 3 Reflection

1. **What was the most confusing thing about Python compared to JavaScript?**

   The biggest adjustment was how Python handles structure and data. In JavaScript I’m used to curly braces for blocks and `array.map()` for transforming lists; in Python, indentation *is* the syntax, and list comprehensions like `[b.upper() for b in books]` felt like a new language at first. I also kept reaching for `python` or global `uvicorn` and hitting the wrong environment—whereas in Node I’m more used to everything living in one `node_modules` folder. Pydantic models were another shift: they feel like TypeScript interfaces, but they actually *run* at request time and reject bad JSON before my route code runs.

2. **What does an HTTP status code tell you? Give one example.**

   A status code tells you the **outcome of the request**—whether it succeeded, failed, and roughly why—without reading the whole response body. For example, when I `POST` a new book and it’s created successfully, the API returns **201 Created**. That means “a new resource was made,” which is more specific than **200 OK**. If I request `GET /books/999` and that id doesn’t exist, **404 Not Found** tells the client the resource isn’t there, not that the server crashed.

3. **What was the difference between a path parameter and a query parameter?**

   A **path parameter** is part of the URL path and usually identifies *one specific resource*. In my API, `GET /books/3` uses `book_id = 3` in the path to fetch that exact book. A **query parameter** comes after `?` and is optional—used for filtering or options, not identity. For example, `GET /books?status=reading` returns all books where `status` is `"reading"`, but the path is still `/books`. I also learned that `/books/stats` must be defined *before* `/books/{book_id}` so FastAPI doesn’t treat `"stats"` as an id.

4. **What would happen to all the data if you restarted the server right now? Why is that a problem, and what will we use to fix it?**

   If I restart Uvicorn right now, **all books disappear**. They only live in the in-memory list `books_db`, which is recreated empty every time the process starts. That’s a problem for a real app: users expect their data to survive deploys, crashes, and restarts. To fix it, we’ll use a **database** (likely SQLite or PostgreSQL) so books are stored on disk or in a persistent service, and the API will read/write through that instead of a Python list in RAM.
