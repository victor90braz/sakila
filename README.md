**Title: Collection of SQL Statements, Laravel Schema Definitions, and Instructions for One-to-One Relationships in Laravel Models**

GitHub Repository: https://github.com/victor90braz/sakila.git

**Database Table Alterations:**

```sql
ALTER TABLE `comments`
ADD FOREIGN KEY (post_id) REFERENCES posts(id);

ALTER TABLE `comments`
ADD FOREIGN KEY (post_id) REFERENCES post_id(id) ON DELETE CASCADE;

ALTER TABLE `comments`
ADD FOREIGN KEY (post_id) REFERENCES post_id(id) ON DELETE RESTRICT;

ALTER TABLE `comments`
ADD FOREIGN KEY (post_id) REFERENCES post_id(id) ON DELETE NO ACTION;

ALTER TABLE `comments`
ADD FOREIGN KEY (post_id) REFERENCES post_id(id) ON DELETE SET NULL;
```

**Database Schema:**

```php
Schema::create('comments', function (Blueprint $table) {
    $table->bigIncrements('id');
    $table->text('body');
    $table->unsignedBigInteger('post_id');
    $table->unsignedBigInteger('user_id');
    $table->timestamps();

    $table->foreign('post_id')->references('id')->on('posts')->onDelete('cascade');
});
```

**SQL JOIN Queries:**

```sql
SELECT * FROM `posts` JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;

SELECT * FROM `posts` INNER JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;

SELECT * FROM `posts` LEFT JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;

SELECT * FROM `posts` RIGHT OUTER JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;
```

**SQL JOIN Notes:**

-   "LEFT OUTER JOIN" gets all records from `posts`.
-   "RIGHT OUTER JOIN" gets all records from `comments`.

**SQL Queries:**

```sql
SELECT * FROM users;

SELECT users.email, users.`name`, users.remember_token FROM users;
```

**SQL GROUP BY Query:**

```sql
SELECT users.email, users.`name`, users.remember_token, COUNT(comments.post_id)
FROM users
LEFT JOIN `comments` ON `comments`.post_id = post_id
GROUP BY users.email, users.`name`, users.remember_token;
```

**One-to-One Relationship:**

**CREATE DB RELATIONSHIP ONE-TO-ONE**

**Schema:**

```php
public function up(): void
{
    Schema::create('profiles', function (Blueprint $table) {
        $table->bigIncrements('id');
        $table->unsignedBigInteger('user_id');
        $table->string('website');
        $table->string('github');
        $table->string('twitter');
        $table->timestamps();

        $table->foreign('user_id')->references('id')->on 'users')->onDelete('cascade');
    });
}
```

**One-to-One Relationship Steps:**

**User Model:**

```php
class User extends Authenticatable
{
    use HasFactory;

    public function profile() {
        return $this->belongsTo(Profile::class);
    }
}
```

**Profile Model:**

```php
class Profile extends Model
{
    use HasFactory;
    protected $guarded = [];

    public function user() {
        return $this->belongsTo User::class;
    }
}
```

\*\*Note: `protected $guarded = [];` should be added to the `Profile` model.

**Tinker Usage:**

Using Laravel Tinker:

```bash
php artisan tinker
$user = new User
$user->first()
$user->first()->profile
$user->first()->toArray()
```

**Note:**

To automatically include the `profile` relationship in queries, you can add `protected $with = ['profile']` in the `User` model.

```php
class User extends Authenticatable
{
    use HasFactory;
    protected $with = ['profile']

    public function profile() {
        return $this->belongsTo(Profile::class);
    }
}
```

Tinker Usage:

```bash
php artisan tinker
$user = new User
$user->all()
$user->first()->toArray()
```

**SELECT \***

```sql
SELECT * from posts
JOIN users on users.id = posts.user_id
WHERE user_id = 1
```

**SELECT posts.id, title, body, published_at, users.name**

```sql
SELECT posts.id, title, body, published_at, users.name from posts
JOIN users on users.id = posts.user_id
WHERE user_id = 1
```

**If we change the function name, specify the user_id as reference**

```php
class Post extends Model
{
use HasFactory;

    public function author() {

        return $this->belongsToMany(User::class, 'user_id');
    }

}
```

**Tinker**

```bash
$user = User::find(1);

> $post = $user->posts()->create([
>
> > . 'title' => 'foo',
> > . 'body' => 'foo baz',
> > . ]);

$user->first()->posts->toArray()
```

**Many to many**

```php
$this->belongsToMany(Post::class);
$this->belongsToMany(Tag::class);

    Schema::create('post_tag', function (Blueprint $table) {
        $table->bigIncrements('id');
        $table->unsignedBigInteger('post_id');
        $table->unsignedBigInteger('tag_id');
        $table->timestamps();

        $table->foreign('post_id')->references('id')->on('posts')
        ->onDelete('cascade');

        $table->foreign('tag_id')->references('id')->on('tags')
        ->onDelete('cascade');
    });
```

```sql
SELECT * from posts
LEFT JOIN post_tag
on post_tag.post_id = posts.id
WHERE post_tag.tag_id = 1

$post = App\Models\Post::find(1)
$post->tags

$post = App.Models.Post.find(6)
post.tags

$tag = App

.Models.Tag.find(2)
$tag->posts
```

Many to Many

```php
$this->belongsToMany(Post::class);
$this->belongsToMany(Tag::class);

Schema::create('post_tag', function (Blueprint $table) {
    $table->bigIncrements('id');
    $table->unsignedBigInteger('post_id');
    $table->unsignedBigInteger('tag_id');
    $table->timestamps();

    $table->foreign('post_id')->references('id')->on('posts')
    ->onDelete('cascade');

    $table->foreign('tag_id')->references('id')->on('tags')
    ->onDelete('cascade');
});

SELECT * from posts
LEFT JOIN post_tag
on post_tag.post_id = posts.id
WHERE post_tag.tag_id = 1

$post = App.Models.Post.find(1)
$post->tags

$post = App.Models.Post.find(6)
$post.tags

$tag = App.Models.Tag.find(2)
$tag->posts
```

**Index**

```sql
EXPLAIN SELECT * from profiles
WHERE user_id = 1

EXPLAIN gives info about the query. If the type is "all," adding an index on user_id can make the search faster. If the type is "ref," the database is already using an index for efficient filtering.

- check "type" and "rows"
```

# example EXPLAIN

EXPLAIN SELECT actor_id, COUNT(\*) from film_actor
GROUP BY actor_id

type(index)
rows(119323)

-----------------// ----------------

SELECT actor_id, COUNT(\*) from film_actor

WHERE actor_id = 5

GROUP BY actor_id

# select

## SELECT \*

SELECT \* from users
LEFT JOIN post_reads
on post_reads.user_id = users.id in (
SELECT id from posts WHERE user_id = users.id
)

## SELECT GROUP BY

SELECT users.id, users.`name` from users
LEFT JOIN post_reads
on post_reads.user_id = users.id in (
SELECT id from posts WHERE user_id = users.id
)

GROUP BY users.id

## SELECT GROUP BY COUNT(\*) as post_read

SELECT users.id, users.`name`, COUNT(\*) as post_read from users
LEFT JOIN post_reads
on post_reads.user_id = users.id in (
SELECT id from posts WHERE user_id = users.id
)

GROUP BY users.id

## SELECT ORDER BY total_post DESC

SELECT users.id, users.`name`, COUNT(\*) as total_post from users
LEFT JOIN post_reads
on post_reads.user_id = users.id in (
SELECT id from posts WHERE user_id = users.id
)

GROUP BY users.id
ORDER BY total_post DESC
