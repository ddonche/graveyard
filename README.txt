# Graveyard

Graveyard is a minimal web application framework for Goblin.

The goal of Graveyard is simple:

```text
request
  ↓
route
  ↓
handler
  ↓
data
  ↓
view
  ↓
response
```

Application code talks to Graveyard.

Application code does not talk directly to database providers.

---

# Installation

Import Graveyard:

```goblin
use graveyard as gy
```

Configure database providers in your application's `graveyard.toml`.

Example:

```toml
[needs.actions]
select = "goblin_supabase::select"
insert = "goblin_supabase::insert"
update = "goblin_supabase::update"
delete = "goblin_supabase::delete"
```

Application code always calls:

```goblin
gy::select(...)
gy::insert(...)
gy::update(...)
gy::delete(...)
```

Never:

```goblin
goblin_supabase::select(...)
```

This allows the underlying database provider to be replaced without changing application code.

---

# Project Layout

A Graveyard application is expected to look like:

```text
my_blog/
  routes.gbln

  handlers/
    posts.gbln

  views/
    posts/
      index.gbln
      show.gbln
      new.gbln

  public/
```

---

# Routes

Routes are declared in `routes.gbln`.

Routes are ordinary Goblin data.

Example:

```goblin
routes | [
    {
        method: "GET",
        path: "/",
        handler: "posts::index"
    },
    {
        method: "GET",
        path: "/posts",
        handler: "posts::index"
    },
    {
        method: "GET",
        path: "/posts/new",
        handler: "posts::new"
    },
    {
        method: "GET",
        path: "/posts/:id",
        handler: "posts::show"
    },
    {
        method: "POST",
        path: "/posts",
        handler: "posts::create"
    }
]
```

The router matches the request and invokes the handler using:

```goblin
:invoke(handler, ctx)
```

---

# Handlers

Handlers live in the `handlers/` directory.

Example:

```goblin
use graveyard as gy

act index(ctx)
    posts | gy::select("posts", {
        order: "created_at.desc"
    })

    html | gy::render("posts/index", {
        posts: posts
    })

    return gy::html(ctx, html)
xx
```

Handlers receive a request context and return a response.

---

# Route Parameters

Routes may capture parameters.

Route:

```goblin
{
    method: "GET",
    path: "/posts/:id",
    handler: "posts::show"
}
```

Request:

```text
/posts/42
```

Handler:

```goblin
id | gy::param(ctx, "id")
```

Value:

```text
42
```

---

# Request Helpers

Graveyard provides:

```goblin
gy::ctx()
gy::method(ctx)
gy::path(ctx)
gy::param(ctx, name)
gy::query(ctx, name)
gy::body(ctx)
gy::header(ctx, name)
```

---

# Database Helpers

Graveyard provides:

```goblin
gy::select(...)
gy::insert(...)
gy::update(...)
gy::delete(...)
```

These delegate through configured action needs.

---

# Rendering

Views live in the `views/` directory.

Example:

```text
views/posts/index.gbln
```

Render:

```goblin
html | gy::render("posts/index", {
    posts: posts
})
```

Graveyard resolves:

```text
views/posts/index.gbln
```

and renders it.

---

# Render Mode

Views begin with:

```goblin
<{ render }>
```

Example:

```goblin
<{ render }>

<h1>Posts</h1>

<{ for post in posts }>
    <article>
        <h2>
            <{ post{"title"} }>
        </h2>

        <p>
            <{ post{"body"} }>
        </p>
    </article>
<{ xx }>
```

Rules:

* Text outside `<{ ... }>` is literal output.
* Code inside `<{ ... }>` is Goblin code.
* Expressions render their value.
* Control structures control output.

---

# Responses

HTML:

```goblin
return gy::html(ctx, html)
```

JSON:

```goblin
return gy::json(ctx, data)
```

Redirect:

```goblin
return gy::redirect(ctx, "/posts")
```

404:

```goblin
return gy::not_found(ctx)
```

403:

```goblin
return gy::forbidden(ctx)
```

---

# Request Lifecycle

```text
HTTP Request
    ↓
Route Match
    ↓
Handler
    ↓
Database Provider
    ↓
View Render
    ↓
Response
```

That is the entire Graveyard model.
