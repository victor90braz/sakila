Collection of SQL statements, Laravel schema definitions, SQL join queries, and instructions for creating and working with one-to-one relationships in Laravel models. I'll keep the information as is, making it easier to reference.

Here is the information you provided:

## Database Table Alterations:

```sql
ALTER TABLE `comments`
ADD FOREIGN key (post_id) REFERENCES posts(id);

ALTER TABLE `comments`
ADD FOREIGN key (post_id) REFERENCES post_id(id) on DELETE CASCADE;

ALTER TABLE `comments`
ADD FOREIGN key (post_id) REFERENCES post_id(id) on DELETE RESTRICT;

ALTER TABLE `comments`
ADD FOREIGN key (post_id) REFERENCES post_id(id) on DELETE no ACTION;

ALTER TABLE `comments`
ADD FOREIGN key (post_id) REFERENCES post_id(id) on DELETE set NULL;
```

## Database Schema:

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

## SQL JOIN Queries:

```sql
SELECT * FROM `posts` JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;

SELECT * FROM `posts` INNER JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;

SELECT * FROM `posts` LEFT JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;

SELECT * FROM `posts` RIGHT OUTER JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;
```

## SQL JOIN Notes:

-   "LEFT OUTER JOIN" gets all queries from `posts`.
-   "RIGHT OUTER JOIN" gets all queries from `comments`.

## SQL Queries:

```sql
SELECT * FROM users;

SELECT users.email, users.`name`, users.remember_token FROM users;
```

## SQL GROUP BY Query:

```sql
SELECT users.email, users.`name`, users.remember_token, COUNT(comments.post_id)
FROM users
LEFT JOIN `comments` ON `comments`.post_id = post_id
GROUP BY users.email, users.`name`, users.remember_token;
```

## One-to-One Relationship:

### CREATE DB RELATIONSHIP ONE-TO-ONE

```sql
SELECT * FROM users
JOIN profiles ON profiles.user_id = user_id;
```

### Schema:

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

### One-to-One Relationship Steps:

#### User Model:

```php
class User extends Authenticatable
{
    use HasFactory;

    public function profile() {
        return $this->belongsTo(Profile::class);
    }
}
```

#### Profile Model:

```php
class Profile extends Model
{
    use HasFactory;
    protected $guarded = [];

    public function user() {
        return $this->belongsTo(User::class);
    }
}
```

#### Note: `protected $guarded = [];` should be added to the `Profile` model.

### Tinker Usage:

Using Laravel Tinker:

```bash
php artisan tinker
$user = new User
$user->first()
$user->first()->profile
$user->first()->toArray()
```

#### Note:

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
```
